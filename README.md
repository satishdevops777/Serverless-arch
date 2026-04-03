# Serverless-arch
- AWS (Amazon Web Services) offers a wide range of serverless services, meaning you don’t have to manage servers—AWS handles provisioning, scaling, and maintenance. These services are typically billed based on usage.
- Here are the main serverless services in AWS, grouped by category:

## 🔧 Compute
**AWS Lambda**
- Run code without provisioning servers. Triggered by events (API calls, file uploads, etc.).
  
**AWS Fargate (serverless containers)**
- Run containers without managing EC2 instances (used with ECS or EKS).

## 🌐 API & Integration
**Amazon API Gateway**
- Create, publish, and manage APIs for backend services (often used with Lambda).
  
**AWS Step Functions**
- Orchestrate workflows by coordinating multiple AWS services.
  
**Amazon EventBridge**
- Event bus for building event-driven architectures.
  
**Amazon SQS (Simple Queue Service)**
- Fully managed message queuing.
  
**Amazon SNS (Simple Notification Service)**
- Pub/sub messaging service.

## 🗄️ Databases
**Amazon DynamoDB**
- Fully managed NoSQL database with on-demand scaling.
  
**Amazon Aurora Serverless**
- On-demand, auto-scaling relational database.
  
**Amazon RDS Proxy (used with serverless apps)**
- Helps manage DB connections for Lambda.

## 📦 Storage
**Amazon S3 (Simple Storage Service)**
- Object storage with no server management.
  
**Amazon EFS (Elastic File System)**
- Serverless file storage that scales automatically.

## 🔍 Analytics & Data Processing
**Amazon Athena**
- Query data in S3 using SQL (no infrastructure).
  
**AWS Glue**
- Serverless ETL (extract, transform, load) service.
  
**Amazon Kinesis (serverless mode)**
- Real-time data streaming and processing.

## 🤖 Machine Learning & AI
**Amazon SageMaker (serverless inference)**
- Build and deploy ML models without managing servers.
  
**AWS Rekognition, Comprehend, Polly, etc.**
- Fully managed AI services with serverless usage.

## 🔐 Security & Identity
**AWS Cognito**
- User authentication and authorization for apps.

## 📊 Monitoring & Management
**Amazon CloudWatch**
- Monitoring, logging, and metrics collection.

**AWS X-Ray**
- Distributed tracing for serverless apps.

## 🧩 Key Characteristics of AWS Serverless
- No server provisioning or management
- Automatic scaling
- Pay-per-use pricing
- Event-driven execution
---

# AWS Labmda
- AWS Lambda = Run code without managing servers
👉 You upload your code → define triggers → Lambda executes it automatically.

## 🚀 1. What is the use of AWS Lambda?
**🔹 Common uses:**
- Backend APIs (with Amazon API Gateway)
- File processing (e.g., image resize on upload to Amazon S3)
- Real-time stream processing
- Automation scripts (cron jobs via Amazon EventBridge)
- Microservices

## ❓ 2. Why do we use Lambda?
**🟢 Key reasons:**
- No server management → no EC2, patching, scaling
- Auto scaling → handles 1 to millions of requests
- Pay per execution → cost-efficient
- Event-driven → integrates with many AWS services
- Faster development → focus only on code

👉 Instead of managing infrastructure, you focus purely on business logic.

## 🔐 3. Security context in Lambda
- Security in Lambda revolves around IAM + execution environment

**🔑 Key components:**
**1. Execution Role (VERY IMPORTANT)**
- Lambda runs using an IAM Role
- This role defines what the function can access
- Example:
```
Read from S3
Write to DynamoDB
Send logs to CloudWatch
```
👉 Principle: Least privilege access

**2. Resource-based Policies**
- Control who can invoke Lambda
- Example:
  - API Gateway invoking Lambda
  - S3 triggering Lambda

**3. Environment Variables Security**
- Store secrets (but use:
- AWS Secrets Manager or
- AWS Systems Manager Parameter Store)

**4. VPC Configuration**
- If Lambda accesses private resources (RDS, internal APIs)
- Attach Lambda to a VPC

**5. Encryption**
- At rest (AWS-managed keys / KMS)
- In transit (HTTPS)


## 💰 4. How Lambda optimizes cost?
**💡 Pricing factors:**
- Number of requests
- Execution duration
- Memory allocated
**🟢 Cost optimization techniques:**
- Choose right memory (affects CPU too)
- Reduce execution time
- Use provisioned concurrency only when needed
- Avoid idle resources (unlike EC2)
- Use event-driven triggers instead of polling

**👉 Example:**
- EC2 runs 24/7 ❌
- Lambda runs only when needed ✅

## 🖥️ 5. AWS Console Options (Lambda)

Here’s what each section means in the Lambda console:

**📊 Dashboard**
- Overview of all Lambda functions
- Metrics: Invocations, Errors, Duration
- Quick monitoring
  
**📦 Applications**
- Pre-built serverless apps (via AWS SAM / Serverless repo)
- Helps deploy ready solutions quickly

**⚙️ Functions (Core area)**
- Where you create/manage functions
- Inside a function:
  - Code → upload or write code
  - Test → simulate events
  - Monitor → logs (CloudWatch)
  - Configuration:
    - Memory
    - Timeout
    - Environment variables
    - Triggers
    - Permissions (IAM role)
      
**📚 Additional Resources**
- Documentation links
- Learning resources

**⚡ Capacity Providers (New)**
- Used mainly with container workloads (less relevant for basic Lambda)
- Helps manage compute capacity

**🔏 Code Signing Configurations**
- Ensures only trusted code is deployed
- Important for security compliance

**🔗 Event Source Mappings**
- Connect Lambda to:
  - SQS, Kinesis, DynamoDB streams
  - Controls batch size, retries

**🧱 Layers**
- Reusable code packages (libraries, SDKs)
- Example:
  - Common utilities
  - ML libraries

**🌍 Replicas**
- For Lambda@Edge / global replication scenarios

**🔗 Related AWS Resources**
- Quick links to:
  - CloudWatch logs
  - IAM roles
  - Triggers
  
**🔄 Step Functions State Machines**
- Integration with AWS Step Functions
- Used for workflows involving multiple Lambdas

# Projects
---

## 🏦 PROJECT 1: Real-Time Transaction Monitoring (Fraud Alert System)

### 🎯 What this project does
- A bank monitors transactions in real time and flags suspicious ones (e.g., high-value transfers).
- 👉 Example: Transaction > ₹10 lakh → trigger alert

### 🧠 How it works (real flow)
```
Transaction → Stream → Lambda → Check rules → Alert / Store
```
**🧰 Services used**
- Amazon Kinesis, AWS Lambda, Amazon SNS, Amazon CloudWatch

### ❓ Why these services
- Kinesis → real-time streaming
  👉 “Streaming” = continuous flow of small data events
  In banking, each transaction = one event
  ```JSON
  { "id": "txn1", "amount": 5000 }
  { "id": "txn2", "amount": 2000000 }
  { "id": "txn3", "amount": 100 }
  ```
  🔄 Real Production Flow
  ```
  Payment system → Kinesis → Lambda → Fraud check
  ```
  👉 In real bank:
  ```
  ATM
  UPI
  Card swipe
  All push events into Kinesis
  ```
- Lambda → instant processing
- SNS → alerting
- CloudWatch → monitoring

### ⚙️ Auto or Manual?
- 👉 Fully automatic: As soon as data comes → Lambda runs

**🔐 IAM Role**
- Attach to Lambda: Kinesis read, SNS publish, CloudWatch logs

### 🛠️ AWS Console Setup
```
Step 1: Create Kinesis Stream
- Name: txn-stream
Step 2: Create SNS Topic
- Name: fraud-alerts
- Add email subscription
Step 3: Create Lambda
- Runtime: Python
- Name: fraud-checker
Step 4: Add Trigger
- Select Kinesis → txn-stream
```
- Lambda Code
```py
import json
import base64
import boto3

sns = boto3.client('sns')
TOPIC_ARN = "YOUR_SNS_ARN"

def lambda_handler(event, context):
    for record in event['Records']:
        payload = base64.b64decode(record['kinesis']['data'])
        data = json.loads(payload)

        if data['amount'] > 1000000:
            sns.publish(
                TopicArn=TOPIC_ARN,
                Message=f"Fraud Alert: {data}"
            )
```

### 🧪 Testing
- Send test data:
```
{
  "id": "txn101",
  "amount": 1500000
}
```
Verify:
- Email alert received
- Logs in CloudWatch
  
