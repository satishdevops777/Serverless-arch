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

Kinesis → Lambda → 
   → DynamoDB (store result)
   → SNS (alert)
   → DLQ (failures)
   → CloudWatch (monitoring)
```
**🧰 Services used**
- Amazon Kinesis, AWS Lambda, Amazon DynamoDB, Amazon SNS, Amazon SQS, Amazon CloudWatch

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
Flow:
```
Transaction → Kinesis → Lambda → (if suspicious) → SNS Email Alert
```

### ✅ STEP 1: Create Kinesis Stream
- Go to Kinesis → Data streams → Create data stream
- Fill:
```
Name: txn-stream
Capacity mode: On-demand ✅
Max record size: 1024 (default)
```
- Expand Server-side encryption → Enable (AWS managed key) ✅
- Click Create

⏳ Wait until Status = Active


### ✅ STEP 2: Create SNS Topic (for alerts)
- Go to SNS → Topics → Create topic
- Choose:
```
Type: Standard
Name: fraud-alerts
```
- Enable encryption → AWS managed key 
- Create topic
```
Add email subscription:
Open topic → Create subscription
Protocol: Email
Endpoint: your email
Confirm from your inbox
```


### ✅ STEP 3: Create DynamoDB Table
- Go to DynamoDB → Create table
- Table name: transactions
- Partition key: transactionId
- Create

### ✅ STEP 4: Create SQS DLQ (Failure Handling)
- Go to SQS → Create queue
- Type: Standard
- Name: fraud-dlq

### ✅ STEP 5: Create IAM Role for Lambda
- Go to IAM → Roles → Create role
- Trusted entity: AWS service → Lambda
- Attach policies:
```
AWSLambdaBasicExecutionRole (for logs)
```
- Click Next → Create
- Add inline permissions (important):
- Open role → Add permissions → Create inline policy → JSON:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "kinesis:GetRecords",
        "kinesis:GetShardIterator",
        "kinesis:DescribeStream"
      ],
      "Effect": "Allow",
      "Resource": "*"
    },
    {
      "Action": "sns:Publish",
      "Effect": "Allow",
      "Resource": "*"
    },
    {
      "Action": "dynamodb:PutItem",
      "Effect": "Allow",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "sqs:SendMessage",
      "Resource": "arn:aws:sqs:us-east-1:713605904859:fraud-dlq"
    }
  ]
}
```

### ✅ STEP 6: Create Lambda Function
- Go to Lambda → Create function
- Choose:
```
Author from scratch
Name: fraud-checker
Runtime: Python 3.x
Execution role: Use existing role → select your role
```
- Create function

### ✅ STEP 7: Add Kinesis Trigger
- Open your Lambda → Add trigger
- Select:
```
Source: Kinesis
Stream: txn-stream
Starting position: Latest
Batch size: 10
```
- Click Add

### ✅ STEP 8: Add Lambda Code
- Replace code with:
```py
import json
import base64
import boto3
import uuid

# AWS clients
sns = boto3.client('sns')
dynamodb = boto3.resource('dynamodb')
sqs = boto3.client('sqs')

# Config
TABLE_NAME = "transactions"
TOPIC_ARN = "arn:aws:sns:us-east-1:713605904859:fraud-alerts"
DLQ_URL = "https://sqs.us-east-1.amazonaws.com/713605904859/fraud-dlq"

table = dynamodb.Table(TABLE_NAME)


def lambda_handler(event, context):
    for record in event['Records']:
        data = None  # ✅ prevent undefined error

        try:
            # Decode Kinesis data
            payload = base64.b64decode(record['kinesis']['data'])
            data = json.loads(payload)

            print("Received:", json.dumps(data))

            # ✅ Validate required fields
            if 'amount' not in data:
                print("Missing amount → DLQ:", data)
                send_to_dlq(data)
                continue

            # Transaction ID
            transaction_id = data.get('id', str(uuid.uuid4()))

            # ✅ Safe amount parsing
            try:
                amount = int(data['amount'])
            except Exception:
                print("Invalid amount → DLQ:", data)
                send_to_dlq(data)
                continue

            # Fraud detection
            fraud = amount > 1000000

            # Store in DynamoDB
            table.put_item(Item={
                'transactionId': transaction_id,
                'amount': amount,
                'fraud': fraud
            })

            print(f"Stored transaction: {transaction_id}, amount: {amount}, fraud: {fraud}")

            # Send alert if fraud
            if fraud:
                sns.publish(
                    TopicArn=TOPIC_ARN,
                    Message=json.dumps({
                        "alert": "Fraud detected",
                        "transaction": data
                    })
                )
                print("Fraud alert sent!")

        except Exception as e:
            print("Critical error:", str(e), "Data:", data)

            # ✅ Send only if data exists
            if data:
                send_to_dlq(data)

            continue


def send_to_dlq(data):
    try:
        sqs.send_message(
            QueueUrl=DLQ_URL,
            MessageBody=json.dumps(data)
        )
        print("Sent to DLQ:", json.dumps(data))
    except Exception as e:
        print("DLQ send failed:", str(e))
```


👉 Click Deploy

### ✅ STEP 9: Configure DLQ
- Lambda → Configuration
- Asynchronous invocation
- Destination → SQS → fraud-dlq

### 🧪 STEP 7: RUN / TEST (Very Important)
- Option A: AWS Console (quick test)
- Go to Kinesis → txn-stream
- Click Put record
- Enter:
```
Partition key: key1
Data:
{"id":"txn101","amount":1500000}
```
- Click Put record

### 🔄 What happens now
```
Data enters Kinesis
Lambda triggers automatically
Data stored in DynamoDB
SNS sends alert email
Logs go to CloudWatch
```
### ✅ STEP 10: VERIFY
1. Check Lambda logs

Go to:
```
Amazon CloudWatch → Logs → /aws/lambda/fraud-checker
```
You should see:
```
Received: {...}
Fraud detected!
```

2. Check Email
- You should receive Fraud Alert email

3. Check Metrics
- Lambda → Monitor tab
- Invocations should increase

4. DynamoDB: Record stored

5. ✔ DLQ (failure test)
- Send invalid data:
```
{
  "amount": "bad-data"
}
```
- Check SQS → message appears

### 🔐 SECURITY (Production Level)
```
Enable encryption everywhere
Use IAM least privilege
No hardcoded secrets
```
### 💰 COST OPTIMIZATION
```
Lambda → pay per use
DynamoDB → on-demand
Kinesis → on-demand
```
### 🧠 SRE VALIDATION CHECKLIST
```
✔ Auto-trigger working
✔ Alerts working
✔ Data stored
✔ Failures handled
✔ Logs visible
```

### ⚠️ Troubleshooting (very common)
- No Lambda trigger: Check Event source mapping enabled
- No email: Confirm SNS subscription
- Permission error: Check IAM role (sns:Publish + kinesis read)
- No logs: Check CloudWatch permissions

***Note: You DO NOT run Lambda manually***
```
It runs automatically when:
Data enters Kinesis
```

| Responsibility   | Service    |
| ---------------- | ---------- |
| Ingestion        | Kinesis    |
| Processing       | Lambda     |
| Storage          | DynamoDB   |
| Notification     | SNS        |
| Monitoring       | CloudWatch |
| Failure handling | SQS DLQ    |

- In this architecture, each AWS service plays a specific role to build a reliable, scalable, and event-driven system. Amazon Kinesis is used as the entry point to ingest continuous real-time transaction data. AWS Lambda processes each transaction automatically without managing servers and applies fraud detection logic. The results are stored in Amazon DynamoDB, which provides fast and scalable storage for auditing and future analysis. If any suspicious activity is detected, Amazon SNS sends instant alerts such as emails or notifications. Amazon CloudWatch is used to monitor logs, metrics, and system health, helping in debugging and observability. Finally, Amazon SQS (used as a Dead Letter Queue) ensures that failed events are not lost and can be retried later, making the system fault-tolerant. Together, these services form a fully automated, scalable, and production-ready solution for real-time transaction processing.
