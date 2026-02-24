# AWS Lambda (Serverless Compute)

AWS Lambda is a fully managed, serverless compute service provided by Amazon Web Services (AWS) that allows you to run code without provisioning or managing servers.

 You upload your code, define triggers, and AWS handles:

- Server management
- Scaling
- High availability
- Fault tolerance

You are charged only for the compute time your code actually uses.

## 1. What is Serverless?

“Serverless” does not mean there are no servers.
It means you don’t manage them.

AWS handles:

- Infrastructure
- OS patches
- Scaling
- Capacity planning

You focus only on:

- Writing business logic
- Connecting services

<br/>

## 2. Key Features of AWS Lambda

- Fully managed compute
- Event-driven execution
- Automatic scaling
- Pay-per-use pricing model
- Built-in fault tolerance
- Supports multiple programming languages
- Integrates with 200+ AWS services

<br/>

## 3. Supported Runtimes

Lambda supports:
- Python
- Node.js
 - Java
- Go
- Ruby
- .NET

Custom Runtime (using Runtime API)

Container Images (up to 10GB)

## 4. Core Components of Lambda
### 4.1 Function

The actual code that performs a task.

Example tasks:

- Process image upload
- Handle API request
- Send email
- Transform data

### 4.2 Trigger (Event Source)

An event that invokes your function.

Common triggers:
- Amazon S3 (file upload)
- API Gateway (HTTP request)
- DynamoDB Streams
- CloudWatch Events
- SNS
- SQS
- EventBridge

### 4.3 Execution Role (IAM Role)

An IAM role that gives permission to Lambda to access AWS services.

Example permissions:

- Read from S3
- Write to DynamoDB
- Send logs to CloudWatch

Without correct permissions → your function fails.

## 5. How Lambda Works (Execution Flow)

Event occurs (e.g., file uploaded to S3)

Trigger sends event data to Lambda

Lambda creates execution environment

Code runs

Response returned

Environment may be reused (warm start)

## 6. Creating Your First AWS Lambda Function
**Step 1: Navigate**

AWS Console → Services → Lambda → Create Function

**Step 2: Choose Creation Method**

- Author from scratch
- Use Blueprint
- Container Image

Select: Author from scratch

**Step 3: Basic Configuration**

- Function name

- Runtime (e.g., Python 3.12)

- Architecture (x86_64 or arm64)

- Execution Role:
Create new role with basic Lambda permissions


**Step 4: Write Code**

Example (Python):



```python
import boto3
region ='region'
instances =['instance id' ]
ec2=boto3.client('ec2',region_name=region)

def lambda_handler(event,context):
    ec2.start_instances(InstanceIds=instances)
    print('Started your instances:'+str(instances))

```
`This practical code can be used to Start the EC2 instance that's already been created!`

You can:
- Use inline editor
- Upload .zip file
- Deploy container image

**Step 5: Deploy**

Click Deploy.

Step 6: Test Function

Click Test

Create new test event


Add JSON:
```json
{
  "key": "value"
}

```
Run test

Check execution result

## 7. Lambda Configuration Options
Memory Allocation:
128 MB to 10,240 MB

CPU power increases with memory

More memory = faster execution (sometimes cheaper)

Timeout

- Default: 3 seconds
- Maximum: 15 minutes

If execution exceeds timeout → function fails

*Environment Variables*

Used to store:
- API keys
- Database URLs
- Config values



## 8. Cold Start vs Warm Start
**Cold Start**

A cold start happens when:
- Lambda runs for the first time
- Or after being idle
- Or when scaling creates a new execution environment

During cold start:
- AWS provisions container
- Loads runtime
- Initializes your code

Cold starts are slightly slower.

<br/>

**Warm Start**

If Lambda reuses an existing execution environment:

- No re-initialization
- Faster response
- Lower latency
<hr>

## 9. Concurrency & Scaling

AWS Lambda automatically scales based on incoming requests.

**Concurrency**

Concurrency = number of function instances running at the same time.

Example:

- 1000 requests at same time → 1000 concurrent executions

*Default account limit: 1000 concurrent executions (can be increased).*

<hr/>

**Reserved Concurrency**

Guarantees a specific number of executions for a function.

Useful when:
- You want to protect critical functions
- Prevent other functions from consuming all concurrency

<hr> 

**Provisioned Concurrency**

Keeps function instances warm.

Used when:
- You need consistent low latency
- nAvoid cold starts in production APIs

<hr> 

## 10. Lambda Pricing Model

You pay for:

1. Number of requests
2. Duration (in milliseconds)
3. Allocated memory

Pricing is calculated as:

`Requests × Duration × Memory`

Free Tier (per month):

- 1 million requests
- 400,000 GB-seconds compute time

<hr>

## 11. Monitoring & Logging

AWS Lambda integrates with:

- Amazon CloudWatch Logs
- CloudWatch Metrics
- AWS X-Ray (for tracing)

CloudWatch automatically records:
- Invocation count
- Errors
- Duration
- Throttles
- Concurrent executions

You can add logs using:
```python
print("Log message")
```
Logs appear in CloudWatch.

<hr>

## 12. Error Handling & Retries

Lambda supports two invocation types:

**Synchronous Invocation**

Example: API Gateway

- Client waits for response
- No automatic retry


**Asynchronous Invocation**

Example: S3, SNS

- Lambda retries automatically (2 times by default)
- Can configure Dead Letter Queue (DLQ)


**Dead Letter Queue (DLQ)**

If function fails after retries:
- Event sent to SQS or SNS
- Helps debugging failed events

<hr>

## 13. Lambda Layers

Lambda Layers allow you to:

- Share libraries
- Separate dependencies
- Reduce package size
- Reuse common code

Example use cases:

- Database drivers
- Utility libraries
- Common logging modules

<hr>

## 14. Lambda with VPC

By default, Lambda runs outside your VPC.

If you need to connect to:
- RDS
- Private EC2
- Internal services

You must configure:

- VPC
- Subnets
- Security Groups

**Note:**
Improper VPC configuration can increase cold start time.

<hr>

## 15. Security Best Practices

- Use IAM Least Privilege principle
- Store secrets in AWS Secrets Manager
- Enable encryption at rest
- Use environment variables securely
- Rotate credentials regularly
- Enable CloudTrail for auditing

<hr>

## 16. Deployment Methods

You can deploy Lambda using:
- AWS Console
- AWS CLI
- AWS SAM (Serverless Application Model)
- AWS CDK
- CloudFormation
- Terraform
- CI/CD pipelines (GitHub Actions, CodePipeline)

For production systems → avoid manual console deployment.

<hr>


## 17. When NOT to Use Lambda

Avoid Lambda if:

- Execution time > 15 minutes
- Heavy CPU/GPU workloads
- Long-running processes
- Stateful applications
- Persistent WebSocket servers

Better alternatives:

- EC2
- ECS
- EKS
- Fargate

<hr>

## 18. Real-World Use Cases

- Serverless REST APIs
- File processing pipelines
- Scheduled cron jobs
- Event-driven automation
- Chatbots
- Real-time notifications
- Microservices architecture

<hr>

## 19. Frequently Asked Questions (FAQs)
**Q1. What is maximum execution time?**

15 minutes per invocation.

**Q2. Does Lambda scale automatically?**

Yes. It scales based on incoming traffic.

**Q3. What is a cold start?**

Delay caused when Lambda initializes a new execution environment.

**Q4. Can Lambda connect to a database?**

Yes. It can connect to:

- RDS

- DynamoDB

- Aurora

- External databases

**Q5. What happens if Lambda fails?**

Retry (async events)

Log error in CloudWatch

Send event to DLQ

**Q6. What is the maximum deployment package size?**

ZIP upload: 50 MB (direct)

Unzipped: 250 MB

Container image: 10 GB

**Q7. Can Lambda run continuously?**

No. Maximum 15 minutes per execution.

**Q8. Is Lambda suitable for microservices?**

Yes. It is widely used for serverless microservices.

**Q9. Does Lambda support environment variables?**

Yes. Used for configuration and secrets.

**Q10. Is Lambda expensive?**

For low to medium workloads → very cost-effective.
For heavy constant workloads → EC2 may be cheaper.














