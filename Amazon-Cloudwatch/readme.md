# AWS Cloud Infrastructure & Deployment Guide

This document outlines the end-to-end process of setting up a VPC, deploying a static site on EC2, scaling with Load Balancers, and monitoring with CloudWatch.

---

## 1. AWS EC2 Deployment
- **Launch Instance:** Select **Ubuntu** as the Amazon Machine Image (AMI).
- **Key Pair:** Create a new key or use an existing one (Type: **RSA**, Format: **.pem**). This acts as your secure password.
- **Security Inbound Rules:** Ensure you open **Port 80 (HTTP)** and **Port 443 (HTTPS)** via the Security Group wizard to allow web traffic.

### Commands for Deploying Static Site
```bash
# Update system and install web server
sudo apt update
sudo apt install nginx -y

# Verify installation
nginx -v

# Clone project from GitHub
git clone https://github.com

# Deploy files to the web server root
sudo cp -r ./repo_name/* /var/www/html/

# Verify history of commands
history

```

Your site is now live at the Public IP address of the instance.

---

## 2. Creating Images (AMI)

Images act as "**blueprints**" to handle more load or provide backups.

Action: Right-click deployed **instance** > **Image and templates** > **Create image.**

Purpose: Launch identical instances instantly without re-configuring software.

Launch: Use your previous `.pem` key for access during the new setup.

---
## 3. Storage: Elastic Block Storage (EBS)

Use EBS when extra disk space (volumes) is required.

Create: Go to **EC2 Dashboard** > **EBS** > **Volumes** > Create Volume.

Tagging:

Key: `Name`

Value: `Volume_name`

Attach: Select **volume** > **Actions** > Attach Volume > Select your instance.

Note: Volumes can be detached safely when no longer needed.

---
## 4. Auto Scaling & Load Balancer

A. Launch Template

Create a template based on your AMI.

Enable Auto Scaling Guidance.

Use your existing *Key Pair*.

Set network fields to "Don't include in template" so the Auto Scaling Group manages placement.

<br/>

B. Auto Scaling Group (ASG)

Select the Launch Template created above.

Availability Zones: Select two or more AZs to ensure high availability.

<br/>

C. Load Balancer Integration

Type: Select Application Load Balancer (ALB).

Scheme: Set to Internet-facing for public access.

Listeners & Routing: Create a new Target Group to route traffic.

Health Checks: Enable them so the balancer only sends traffic to "healthy" instances.

<br/>

D. Group Size & Scaling Policy

Desired Capacity: Number of instances to run normally (e.g., 1 or 2).

Min/Max: Set your scaling boundaries.

Policy: Use Target Tracking (e.g., scale up if CPU usage exceeds 70%).

---
## 5. Amazon CloudWatch Monitoring

Track health and performance in real-time.

A. Monitoring Metrics
CPU Utilization: Check if instances are under heavy load.

Status Checks: Alerts for hardware or system failures.

Billing Alarms: Monitor costs to avoid surprises.

<br/>

B. Setting Up Alarms
Go to CloudWatch > **Alarms** > **Create Alarm.**

Select metric: **EC2** > **Per-Instance Metrics** > **CPUUtilization.**

Define threshold: (e.g., "Greater than 80% for 2 minutes").

Action: Configure an SNS (Simple Notification Service) topic to send an email alert.

---
# FAQ
**Q1:** Can we connect a "Volume" and "Instance" in different Availability Zones?

**Ans:** No. The AZ must be the same. Like a pendrive and a laptop, they must be in the same physical location to connect.

<br/>

**Q2:** Can we connect multiple Volumes to one Instance?

**Ans:** Yes. You can attach multiple EBS volumes to a single instance for different storage needs.

<br/>

**Q3:** Why select more Availability Zones while load balancing?

**Ans:**
Outage Isolation: If one data center fails, your site stays live in the other.
Even Traffic: Distributes requests to prevent any single zone from bottlenecking.
AWS Best Practice: ALBs require at least two AZs to ensure proper fault tolerance.

