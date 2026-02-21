# 1. AWS EC2

- Create instance, using 'launch instance'
- put details
- select 'key' (create new or use old, its like a password to your instance).
- Key is RSA type and save as **'.pem'**
- system as ubuntu

## Commands for deploying static site
```code
sudo apt update
sudo apt install nginx
nginx -v
git clone http://github.com/your_profile/repo_name.git

sudo cp -r ./repo_name/* /var/www/html
history
```

 ~now this site is deployed;

## Security Inbounds
  set http and https in inbound setting
  go to wizard for it.

# 2. Creating Image (AMI)
  We create images, when there is more load. So we can divert the traffic. (although for static sites, AWS Auto Scaling is used. For automatic creations, and also Kubernetes EKS)

  - right click on deployed instance
  - create new image.
  - launch new instance
  - edit the config for all access directly there
  - use the previous key, else create new
  - done


# 3. Creating Volume and attaching it

if more volume is needed, We use Elastic Block Storage (EBS).

- go to EBS, create volume
- select the configuration as per need;
- In Tags sections ~ Provide **key** as *'Name'* and then **Value** as *"Volume_name"*;
- create volume
- select the volume and then **Attach** it to needed instance;

After the use user can *detach* the volume.

# 4. AWS Auto Scaling and Load Balancer

A. First we select instance we need to scale
   - select image or instance which needs to be scaled.
   - make template of image,
   - enable *Auto guidance*
   - provide **Key** previously made or a new one.
   - Let other fields be **Don't include in launch tempelate**.
   - Done.
     
B. Second Step is Creating Auto Scaling Group
   1. choose launch template just created, or if have a previously made.
   2. Instance launch option
      - elect availability zone as two or more.
     
C. Integrating other services
   1. Load balancer -> Three option to choose from *No load balancer*, *Existing load balancer*, and *Create new load balancer*.
   2. Put Load balancer name
   3. Select **Internet facing**, access over public internet.
   4. Listeners & routing -> default routing -> **Create new One**.
   5. Health check can be enabled as well;

D. Configure Group size and Scaling
   1. Desired capacity [ 1 or more ]
   2. Scaling, Min and Max as need.
   3. Automatic Scaling -> Target tracking scaling policy.

E. Tags
   Use **Key** as **Name**
   Use **Value** as **InsName_connected**.
   We give similar tag, So later we could understand which instance is getting connected.

# FAQ

## Ques 1:
  Can we connect "volume" and "Instance" of different availability zone?
## Ans 1:
  No, we can't. The Availability zone must be same.
  Think of it like pendrive and laptop, if both of them are present in different places then its impossible to connect them.

## Ques 2:
  Can we connect multiple Volume to one instance?
## Ans 2:
  Yes, we can connect multiple to one instance.

## Ques 3:
  Why select more Availability zone while load balancing
## Ans 3:
  Selecting multiple Availability Zones (AZs) is a foundational best practice for **High Availability** and **Fault Tolerance.** 
  - Isolation from Outages: Each AZ is a physically separate data center with independent power, cooling, and networking.      By spreading your instances across multiple AZs, your application stays online even if one entire data center fails.
  - Even Traffic Distribution: Load balancers use Cross-Zone Load Balancing to distribute incoming requests evenly across      all  healthy instances in all selected AZs. This prevents one zone from being overwhelmed while another sits idle.
  - Application Load Balancer (ALB) Requirement: For an ALB, AWS actually requires you to enable at least two Availability     Zones to ensure it can continue routing traffic if one zone becomes unavailable. 
  
  
