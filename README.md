# AWS DVA-C01 Cheat-sheet

## AWS S3

- **Enabling Replication** on S3 bucket **increases storage** very quickly/ **Exponentially**
- **CloudFront Distributions** to reduce latency of websites to users
- **Encryptions** :
  - Amazon S3-Managed Keys **(SSE-S3)** : the data is encrypted by Amazon S3 using keys that are managed through S3
  - Server-Side Encryption with Customer-Provided Keys **(SSE-C)** : you manage the encryption keys and Amazon S3 manages the encryption
  - Server-Side Encryption with Customer Master Keys **(CMKs)** Stored in AWS Key Management Service **(SSE-KMS) :** auditing and permissions , control
- Access S3 privately (rather than across the Internet) - Configure a hardware VPN to a VPC and configure an S3 endpoint

## IAM ROLE

- Using IAM Role for EC2, ASG is more secure than providing access via IAM user
- Want EC2 instance to access other AWS services use IAM ROLE

## Application Load Balancer

- Sticky sessions, ( session affinity ) maintain state information , helps to prevent re-authentication
- Target type
  - **Instance** : The targets are specified by instance ID.
  - **IP** : The targets are IP addresses.
  - **Lambda** : The target is a Lambda function.

## CloudFormation

- **Parameters**
  - It&#39;s like variables declaration, helps to input custom values to your template
- **Mappings**
  - set values based on a region
  - use the _ **Fn::FindInMap** _ intrinsic function to retrieve values in a map
- **Resources**
  - This is the **ONLY mandatory field**
  - AWS resources that you want to include in the stack, Ex: EC2 instance or S3 bucket.
- **Outputs**
  - utput values that you can import into other stacks
  - view on the AWS CloudFormation console.

## SAM - Serverless Application Module

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
- Has its own cli (different from aws cli)

## Lambda

- To **increase performance keep connect or close statements outside of the handler** , Place the connection in the global space
- HTTP **Status Code: 429** : _TooManyRequestsException_ ,
  - The request **throughput limit was exceeded**  **for Synchronous invocations**.
  - Asynchronous invocation  retry, then to DLQ (Dead letter Queue)
- You **CANNOT**** configure an event-source mapping between and ALB and a Lambda function ,** instead use ALB target type as lambda
- Configuration Elements – check what does each do ?

### Step Functions

- **Combine Lambda** , or **branching**** Lambda **or** parallel **processing or** Error handling**

![](RackMultipart20210621-4-1061jtj_html_f95860abd88605c8.png) ![](RackMultipart20210621-4-1061jtj_html_b1beb7ee27eb3d43.png) ![](RackMultipart20210621-4-1061jtj_html_9dce2b0f9ec11d91.png) ![](RackMultipart20210621-4-1061jtj_html_62c40b64ebf6b104.png)

- **State machine** (workflow) and tasks

### Lambda Limits

| **Resource** | **Quota** |
| --- | --- |
| Function memory allocation | 128 MB to 10,240 MB, in 1-MB increments. |
| Function timeout | 900 seconds (15 minutes) |
| Function environment variables | 4 KB |
| Function resource-based policy | 20 KB |
| Function layers | 5 layers |
| Function burst concurrency | 500 - 3000 (varies per Region) |
| Deployment package (.zip file archive) size | 50 MB (zipped, for direct upload)
 250 MB (unzipped, including layers)
 3 MB (console editor) |
| Container image code package size | 10 GB |

- **Session data** and Lambda
  - AWS Lambda is a stateless compute service and so you cannot store session data in AWS Lambda itself
  - You can use **DynamoDB or AWS S3** with lambda to store some session data (user data)
- **Concurrency** : is the number of requests that your function is serving at any given time
  - **Concurrency = number of requests \* number of executions per sec.**
  - Ex: if we have 20 requests and each requests takes 20 sec to execute then concurrency = 20\*20=400

## SQS

- AdjustReceiveMessage  parameter if consumer is not able to process incoming messages , ex: setReceiveMessage to 10 to retrieve 10 messages at a time
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

## ECS - Elastic Container Service

- **Task Placement strategies** : Algorithm for selecting instances for task placement or tasks for termination
  - _ **binpack** _
    - **minimizes the number of instances** in use
    - Place tasks based on the **least available amount of CPU or memory**
  - _ **random** _
    - **randomly** place the tasks
  - _ **spread** _
    - Place tasks **evenly based on the specified value** (instanceId)
  - [https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-placement-strategies.html](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-placement-strategies.html)
- Task definitions
-

- ECS Deployment types
  - Rolling update
  - Blue/Green deployment with CodeDeploy
  - External Deployment

## AWS Elastic Beanstalk

- Source bundle:
  - Consist of a **single ZIP file or WAR file** (you can include multiple WAR files inside your ZIP file)
  - **Not exceed 512 MB**
  - **Not include a parent folder or top-level directory** (subdirectories are fine)
- **cron.yaml** : is used when you deploy a **worker application** that processes **periodic background tasks**
- Deployment Policies
  - **All at once** :
    - **Fastest** deployment
    - **No additional Cost**
    - **Complete outage , Needs downtime, all instances are taken down**
    - Deploys the new version to **all instances simultaneously**
  - **Rolling:**
    - Update a few instances at a time (batch), and then move onto the next batch once the first batch is healthy
    - **No additional Cost**
    - Reduced capacity of instance which impacts application performance
  - **Immutable** :
    - Total deployment of new instances
    - **Higher cost** , because you bring up equal number of new instances
  -
  - [https://blog.shikisoft.com/which\_elastic\_beanstalk\_deployment\_should\_you\_use/](https://blog.shikisoft.com/which_elastic_beanstalk_deployment_should_you_use/)
  - [https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html#deployments-scenarios](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html#deployments-scenarios)
  - [https://jayendrapatil.com/aws-elastic-beanstalk-deployment-strategies/](https://jayendrapatil.com/aws-elastic-beanstalk-deployment-strategies/)
  - ![](RackMultipart20210621-4-1061jtj_html_752a966f701e0548.png)
  -

##  Amazon DynamoDB

- [https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/](https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/)
- **Partition key** : Unique identifier, A simple primary key,
- **Composite primary key** _=_ **Partition key and sort key**
- Recommendations for partition keys
- **Use high-cardinality attributes**. These are attributes that have distinct values for each item,
  - _e-mailid, employee\_no, customerid, sessionid, orderid_, and so on.
- Use composite attributes. Try to combine more than one attribute to form a unique key, if that meets your access pattern
- Can **save session state** of the users (but it has **higher latency then Elasticache** )
- Session state with low latency then go for Elasticache and not DynamoDB
- RCU WCU – how to calculate ? : [https://digitalcloud.training/certification-training/aws-developer-associate/aws-database/amazon-dynamodb/](https://digitalcloud.training/certification-training/aws-developer-associate/aws-database/amazon-dynamodb/)
- DynamoDB **Streams** : To keep **records of changes to items in a DynamoDB** table
  - _ **StreamViewType** _ are:
    - _ **KEYS\_ONLY** _ - Only the key attributes of the modified item are written to the stream.
    - _ **NEW\_IMAGE** _ - The entire item, as it appears **after it was modified** , is written to the stream.
    - _ **OLD\_IMAGE** _ - The entire item, as it appeared **before it was modified** , is written to the stream.
    - _ **NEW\_AND\_OLD\_IMAGES** _ - **Both the new and the old item** images of the item are written to the stream.
- Scan and Query
  - Scan queries entire table, use query instead of scans
  - For **Better performance** of (or **reduce impact** on) provisioned throughput, or reduce impact on
    - **Reduce page size**
    - **Isolate scan operations**
  - **Faster scan** results, but will place more burden on the table&#39;s provisioned throughput ( to **reduce the burden**** apply rate limiting**)
    - **parallel Scan (better used in below scenarios)**
      - The table size is 20 GB or larger
      - The table&#39;s provisioned read throughput is not being fully used.
      - Sequential Scan operations are too slow.

## RDS

- RDS is not a key/value store

## ElastiCache

- In-memory caching system , memcached and redis
- When to choose what ?
-

| **Memcached** | **Redis** |
| --- | --- |
| **simplest model** possible | **Snapshots** , automatic failover and backup/restore |
| **Multi-threaded** : Run large nodes with multiple cores or threads | **Replication** |
| ability to scale in and out | **Geospatial support** |
| adding and removing nodes as demand | Complex data structures: sorted sets and lists, Transactions |
| cache objects | Pub/Sub |
|   | Lua scripting |

## Amazon Cognito

- Must be **accessible from**  **mobile** , **desktops** , and **tablets** , and must remember user preferences across platforms
- Supports **unauthenticated access** to users (Cognito Identity Pool)
- provide the sign-up and sign-in functionality to end users with no AWS access

| **Cognito User Pool** | **Cognito Identity Pool (Federated Identities)** |
| --- | --- |
| **Authentication** | **Authorization** |
| sign-up and sign-in webpages for your app | Generate temporary AWS credentials for unauthenticated users. |
| Access and manage user data. | Give your users access to AWS resources , like S3 , dynamoDB |
| Use a custom authentication flow |   |
| Track user device, location, and IP address |   |
| adapt to sign-in requests of different risk levels |   |

- [https://aws.amazon.com/premiumsupport/knowledge-center/cognito-user-pools-identity-pools/](https://aws.amazon.com/premiumsupport/knowledge-center/cognito-user-pools-identity-pools/)

## CodeCommit

## CodeBuild

## CodePipeline

## CodeDeploy

- Can deploy software packages using an archive that has been uploaded to an Amazon S3 bucket

## AWS Secrets Manager

- **Store secrets/credentials** and **automatically rotate them**
- Integration for Amazon RDS, Amazon Redshift, and Amazon DocumentDB

## AWS SSM Parameter

- Can **store secrets** (SecureString) but **NO automatic rotation** , you have to rotate if needed.

## AWS KMS keys

- Region Specific

## CloudWatch

- **CloudWatch Events**
  - Respond to **state changes** in your AWS resources ( useful for triggering lambda func)
- Cloudwatch logs Vs Cloudwatch Events ? – use case example

## CloudTrail

- **Auditing** , Used for logging , continuously monitor, and retain account activity related to actions across your AWS infrastructure , ex : **API activity related to creating, modifying or deleting AWS resources**
- **event history of your AWS account activity** (console, or cli or SDK)

## AWS X-Ray

- **Tracing application activity** for **performance** of applications and operational statistics
- **Segments** : The **data** , like the hostname, alias , IP, start and end times, subsegments , status
- **Subsegments** : **more granular details of segments.**
- **Filter expressions** : find **traces related to specific paths or users**.
- **Annotations** : key-value pair, Indexed, and **used along with Filter expressions**
- Metadata : key value pair, NOT indexed, used for record data you want to store in the trace but don&#39;t need to use for searching traces
- **Errors** : Client errors **(400 series errors)**
- **Faults** : Server faults ( **500 series errors** )
- **Throttle** : Throttling errors ( **429 Too Many Requests** )

- [https://docs.aws.amazon.com/xray/latest/devguide/xray-concepts.html](https://docs.aws.amazon.com/xray/latest/devguide/xray-concepts.html)

## Links

- [https://jayendrapatil.com/aws-certified-developer-associate-june-2018-exam-learning-path/](https://jayendrapatil.com/aws-certified-developer-associate-june-2018-exam-learning-path/)
- Special Thanks to developers of [https://word2md.com](https://word2md.com/) for helping with easy conversation .docx to .md
- [https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/](https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/)