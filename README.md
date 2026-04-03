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
