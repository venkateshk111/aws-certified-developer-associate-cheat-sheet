# aws-certified-developer-associate-cheat-sheet
# AWS DVA-C01 Cheatsheet

## Amazon Cognito

- Must be **accessible from**  **mobile** , **desktops** , and **tablets** , and must remember user preferences across platforms
- Supports **unauthenticated access** to users (Cognito Identity Pool)
- provide the sign-up and sign-in functionality to end users with no AWS access

## AWS S3

- **Enabling Replication** on S3 bucket **increases storage** very quickly/ **Exponentially**
- **CloudFront Distributions** to reduce latency of websites to users

## IAM ROLE

- Using IAM Role for EC2, ASG is more secure than providing access via IAM user

##  Amazon DynamoDB

- [https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/](https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/)
- **Partition key** : Unique identifier, A simple primary key,
- **Composite primary key** _=_ **Partition key and sort key**
- Recommendations for partition keys
- **Use high-cardinality attributes**. These are attributes that have distinct values for each item,
  - _e-mailid, employee\_no, customerid, sessionid, orderid_, and so on.
- Use composite attributes. Try to combine more than one attribute to form a unique key, if that meets your access pattern
- RCU WCU – how to calculate ? : [https://digitalcloud.training/certification-training/aws-developer-associate/aws-database/amazon-dynamodb/](https://digitalcloud.training/certification-training/aws-developer-associate/aws-database/amazon-dynamodb/)
- DynamoDB **Streams** : To keep **records of changes to items in a DynamoDB** table
  - _ **StreamViewType** _ are:
    - _ **KEYS\_ONLY** _ - Only the key attributes of the modified item are written to the stream.
    - _ **NEW\_IMAGE** _ - The entire item, as it appears **after it was modified** , is written to the stream.
    - _ **OLD\_IMAGE** _ - The entire item, as it appeared **before it was modified** , is written to the stream.
    - _ **NEW\_AND\_OLD\_IMAGES** _ - **Both the new and the old item** images of the item are written to the stream.

## AWS SAM - Serverless Application Module

- **Serverless** Application deployments,
- **templatized** serverless application
- **sam package, sam deploy** serverless application
- **CloudFormation package , then CloudFormation deploy**
- **SAM** is extension to CloudFormation , more simpler.
- **Rolls back** the deployment if CloudWatch alarms are triggered
- Deployment Preference Type
  - **Canary** – 2 Increments ( ex: 10% and 90% or 35% and 65%)
  - **Linear** - equal increments , equal min
  - **All-at-once –** All at once.

## SQS

- **Short Polling :**
  - Default SQS behavior , Get msg  send msg (returns immediately) ,
  - Responds even if no msgs in queue. Empty responses possibility is more
  - WaitTimeSeconds =0 or [ReceiveMessageWaitTimeSeconds](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SetQueueAttributes.html) = 0
- **Long Polling :**
  - Wait till some msgs arrive (orReceiveMessageWaitTimeSeconds reaches) in queue then send msg
  - Responds only when at least one msg is in queue , sends an empty response only if the polling wait time expires (max 20sec)
  - helps **reduce the cost** by eliminating the number of empty responses and false empty responses

- **VisibilityTimeout** :
  - Period of time SQS **prevents other consumers from receiving** and processing the message which was already processed, (avoid duplicate processing)
  - **Increase VisibilityTimeout to reduce duplicate msgs being processed**
  - Default is 30sec, min=0sec , max=12hrs
- **Delay Queues:**
  - Postpone the **delivery of new messages to a queue** for a number of seconds
  - Your consumer application needs additional time to process messages
- **VisibilityTimeout** Vs. **Delay Queues**
  - **VisibilityTimeout :** message is consumed once and then timeout is set for other consumers
  - **Delay Queues :** Message itself it delayed to be processed by any consumer for first time itself
- **Dead Letter Queue:**
  - Special queue for messages that can&#39;t be processed (consumed) successfully
  - Helps in debugging your application or messaging system

## AWS Elastic Beanstalk

- Source bundle:
  - Consist of a **single ZIP file or WAR file** (you can include multiple WAR files inside your ZIP file)
  - **Not exceed 512 MB**
  - **Not include a parent folder or top-level directory** (subdirectories are fine)

## Lambda

- To **increase performance keep connect or close statements outside of the handler** , Place the connection in the global space
- HTTP **Status Code: 429** : _TooManyRequestsException_ ,
  - The request **throughput limit was exceeded**  **for Synchronous invocations**.
  - Asynchronous invocation  retry, then to DLQ (Dead letter Queue)
- Configuration Elements – check what does each do ?

### Step Functions

- **Combine Lambda** , or **branching**** Lambda **or** parallel **processing or** Error handling**

![](RackMultipart20210620-4-13qvqy2_html_f95860abd88605c8.png) ![](RackMultipart20210620-4-13qvqy2_html_b1beb7ee27eb3d43.png) ![](RackMultipart20210620-4-13qvqy2_html_9dce2b0f9ec11d91.png) ![](RackMultipart20210620-4-13qvqy2_html_62c40b64ebf6b104.png)

- **State machine** (workflow) and tasks

## AWS Secrets Manager

- **Store secrets/credentials** and **automatically rotate them**
- Integration for Amazon RDS, Amazon Redshift, and Amazon DocumentDB

## AWS SSM Parameter

- Can **store secrets** (SecureString) but **NO automatic rotation** , you have to rotate if needed.

## AWS KMS keys

- Region Specific

## Cloudwatch

- Cloudwatch logs Vs Cloudwatch Events ? – use case example

## AWS SAM

- Has its own cli (different from aws cli)
-

## Application Load Balancer

- Sticky sessions, ( session affinity ) maintain state information , helps to prevent re-authentication

## Links

[https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/](https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/)