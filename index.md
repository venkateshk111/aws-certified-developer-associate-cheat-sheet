# AWS Certified Developer Associate (DVA-C01) Cheat Sheet

1. Following Cheat Sheet was prepared during **June-2021**
2. Not all the DVA-C01 certification services are covered in this cheat sheet, but only which are very important for exam
3. Cheat Sheet was prepared based on
  1. (Mainly) Udemy Practice tests by Jon Bonso , Neal Davis
  2. AWS Documentation
  3. AWS FAQ
  4. AWS Whitepapers
4. This Sheet to be used as last min reference ONLY or for quick reference ONLY
5. This was my notes for last day quick glance before the actual DVA-C01 exam
6. All credits to excellent course by Neal Davis and challenging practice test by Jon Bonso

## Main Course links

1. Neal Davis Course &amp; practice tests :
  1. [https://www.udemy.com/course/aws-certified-developer-associate-exam-training/](https://www.udemy.com/course/aws-certified-developer-associate-exam-training/)
  2. [https://www.udemy.com/course/aws-developer-associate-practice-exams/](https://www.udemy.com/course/aws-developer-associate-practice-exams/)
2. Jon Bonso Practice tests:
  1. [https://www.udemy.com/course/aws-certified-developer-associate-practice-exams-amazon/](https://www.udemy.com/course/aws-certified-developer-associate-practice-exams-amazon/)
3. In Detail Cheat Sheets by Neal and Jon
  1. [https://digitalcloud.training/certification-training/aws-developer-associate/](https://digitalcloud.training/certification-training/aws-developer-associate/)
  2. [https://tutorialsdojo.com/aws-cheat-sheets/](https://tutorialsdojo.com/aws-cheat-sheets/)
4. Other Prominent course for DVA-C01( Stephane Maarek, Ranga Karanam)
  1. [https://www.udemy.com/course/aws-certified-developer-associate-dva-c01/](https://www.udemy.com/course/aws-certified-developer-associate-dva-c01/)
  2. [https://www.udemy.com/course/aws-certified-developer-associate-practice-tests-dva-c01/](https://www.udemy.com/course/aws-certified-developer-associate-practice-tests-dva-c01/)
  3. [https://www.udemy.com/course/aws-certified-developer-associate-step-by-step/](https://www.udemy.com/course/aws-certified-developer-associate-step-by-step/)
  4. [https://www.udemy.com/course/new-exam-review-aws-certified-developer-associate/](https://www.udemy.com/course/new-exam-review-aws-certified-developer-associate/)

## My Preparation for DVA-C01

1. Time : 3-4Weeks (3-4hrs a day)
2. Study Pattern :

- Neal Davis course
- 1 Neal Davis Practice Test
- 4 Jon Bonso Tests
- 2 Stephane Maarek Test
- My Own Notes (This Cheat Sheet)

## My Practice and Actual Exam Results

| **Practice Test** | **Neal** | **Jon** | **Stephane** | **AWS Exam** |
| --- | --- | --- | --- | --- |
| **Practice Test 1** | 60% | 64% | 64% |   |
| **Practice Test 2** |   | 70% | 67% |   |
| **Practice Test 3** |   | 68% |   |   |
| **Practice Test 4** |   | 72% |   |   |
| **DVA-C01** |   |   |   | **89%** |

## Want more tips? Let&#39;s get connected

- Twitter: [https://twitter.com/venkatesh111](https://twitter.com/venkatesh111)
- LinkedIn: [https://www.linkedin.com/in/venkatesh111/](https://www.linkedin.com/in/venkatesh111/)

# DVA-C01 Cheat Sheet

## IAM

- IAM Roles
  - Using IAM Role for EC2, ASG is more secure than providing access via IAM user
  - **ECS tasks can also be assigned with IAM ROLES** just like IAM Role or EC2 instances
  - Want EC2 instance to access other AWS services (Example S3) use IAM ROLE
  - **Cross Account access:**
    - your developers/Ops want to access particular resources in 2 or more different (PROD , TEST) AWS accounts
    - temporary access to resources in a second account (use with STS)
  - **custom identity broker**
    - if your On-Prem LDAP is not compatible with SAML, and you want users to use LDAP to authenticate to AWS use **custom identity brokers**
  - You **cannot attach IAM Role to On-Prem Instances** , use IAM credentials
- **IAM Best Practices**
  - Lock away your AWS account root user access keys
  - Create individual IAM users
  - Enable MFA
  - Use user groups
  - Grant least privilege
  - Use roles for applications that run on Amazon EC2 instances
  - Use roles to delegate permissions
  - [https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

## Application Load Balancer

- Sticky sessions, ( session affinity ) maintain state information , helps to prevent re-authentication
- Target type
  - **Instance** : The targets are specified by instance ID.
  - **IP** : The targets are IP addresses.
  - **Lambda** : The target is a Lambda function.

## S3

- Logging (**server across logging) into the same bucket** causes **exponential log growth**.
- **S3 prefixes**
  - Allows organizing objects, Easily search objects
  - Useful with **IAM and bucket policies to restrict/grant access at object level**
- **Access S3 privately** (rather than across the Internet) - Configure **a hardware VPN to a VPC** and configure an **S3 endpoint**
- **S3 Select**
  - simple structured query language (SQL) statements to filter the contents of Amazon S3 objects and retrieve just the subset of data that you need
  - lower the latency of retrieving data from S3 and reduce costs
- **Encryptions** :
  - Amazon S3-Managed Keys **(SSE-S3)** : the data is encrypted by Amazon S3 using keys that are managed through S3 (AES-256)
    - Request header: _**x-amz-server-side-encryption(AES256)**_
  - Server-Side Encryption with Customer-Provided Keys **(SSE-C)** :
    - You manage the encryption keys and Amazon S3 manages the encryption
    - use HTTPS is mandatory
  - Server-Side Encryption with Customer Master Keys **(CMKs)**
    - Stored in AWS Key Management Service **(SSE-KMS) :**
    - auditing and permissions , control
  - Server side encryption with **AWS KMS (SSE-KMS)**
    - _**x-amz-server-side-encryption(aws:kms)**_ and
    - _**x-amz-server-side-encryption-aws-kms-key-id(ARN for key in KMS)**_
    - **throttles S3 performance** because **AWS KMS API calls has quota limit**
- **S3 Static web hosting**
  - Enable static website option
  - **Disable &quot;Block Public Access&quot;**
  - Configure **Bucket policy to enable public read access**
  - **CloudFront Distributions** to reduce latency of websites to users
- **S3 Replication**
  - To meet regulations, reduces latency
  - Versioning should be enabled on both source and destination bucket
  - Replication can be in same or different region
  - Only new objects are replicated, (explicitly copy the old objects)
  - Ex: To replicate data between dev and test environments
  - **Cross-region replication (CRR)**
    - The source and destination buckets must have **versioning enabled.**
    - The source and destination buckets **must be in different AWS Regions**.
    - Amazon **S3 must have permissions to replicate objects** from that source bucket to the destination bucket on your behalf.
- **Pre-signed URL**
  - **Prevent unauthorized access of your S3 objects** (photos , videos) hosted on S3 static website
  - **Time limited permission** (few hours to 7days)
  - Created using **AWS SDK API**
- **Lifecycle Policies**
  - **Move files automatically**** between storage classes **and** save cost**
  - **Transition: move files** from one class to another
  - **Expiration: Delete** objects

## S3 Glacier

- **S3 Glacier data retrieval options**
  - **Expedited: 1–5 minutes** , **higher-cost**
  - **Standard retrievals** : **3–5 hours**. This is default option.
  - **Bulk retrievals: 5–12 hours** , **lowest-cost** , for large or **petabytes** of data retrieval

## CloudFront

- Lambda@Edge runs your code in response to events generated by the Amazon CloudFront
- use Lambda@Edge to help authenticate and authorize users for the premium pay-wall content on your website, filtering out unauthorized requests before they reach your origin infrastructure
- **Accessing Private content**
  - **Signed URLS**
    - Application/user **downloads single file**
    - Cookies are not supported
  - Signed Cookies
    - Application/user **downloads multiple files**
    - No change in application URLs
- **HTTP 504 errors** , lot t of time to log into their website
  - Use Lambda@Edge
  - set up an **origin failover** by creating an origin group with two origins primary origin and second origin, which CloudFront **automatically switches to when the primary origin fails**
- **Architecture**

1. **Store static content in S3**
2. **Distribute the content** around the world (global users) **using CloudFront**

  - **DO NOT** use it for static content on EC2
- **Advantages**
  - Pay for use of CloudFront , (No charge for data transfer from S3)
  - **Low latency**
  - **Simple caching** with TTL
  - Reduce load on your EC2

- **AWS Shield** :
  - Avoid **DDoS Attacks**
- **AWS WAF** :
  - Protect from **SQL injection** , **Cross-site scripting**

## AWS CLI

- **--dry-run** : to test if you have required permissions or not
- -- **output** : output format (json/yaml/text/table)
- **--page-size** : avoid time out errors
- **--no-paginate** : display only first page
- **--debug** : Debug mode (verbose)

- CLI precedence
  1. **Command line**
  2. **Environment variables**
  3. **CLI credentials file (.aws/config)**
  4. **Container credentials**
  5. **Instance profile credentials**

- [https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-precedence](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-precedence)

## VPC

- **VPC Flow Logs**
  - capture information about the **IP traffic going to and from in your VPC**

## CloudFormation

- **StackSets**
  - **Create, update, or delete stacks**** across multiple accounts **and** regions** with a single operation
  - **Same CloudFormation templates to be used, on different multiple/Different AWS accounts** use StackSets
- **CloudFormation templates**
  - **Metadata**
  - **Parameters**
    1. It&#39;s like variables declaration, contains values to pass to your template at runtime
  - **Rules**
  - **Mappings**
    1. set values based on a region
    2. use the _ **Fn::FindInMap** _ intrinsic function to retrieve values in a map
  - **Conditions**
  - **Transform**
  - **Resources**
    1. This is the **ONLY mandatory field**
    2. AWS resources that you want to include in the stack, Ex: EC2 instance or S3 bucket.
  - **Outputs**
    1. utput values that you can import into other stacks
    2. View on the AWS CloudFormation console.

- **Helper scripts**
  - **cfn-init** : **install packages, create files, and start services**.
  - **cfn-signal** : synchronize other resources in the stack when the prerequisite resource or application is ready.
  - **cfn-get-metadata** : **retrieve metadata** for a resource or path to a specific key.
  - **cfn-hup** : Use to check for updates to metadata and **execute custom hooks** when changes are detected.
- **Cross Stack Reference**
  - Helps you to **simplify cloud formation templates**
  - **!ImportValue**
  - write small scripts for creating SG , subnets, and call them in another script

## SAM - Serverless Application Module

- Build **Serverless**** architecture** in AWS
- Build **Serverless**** Application** deployments,
- **templatized** serverless application, provides **shorthand syntax**
- The declaration  **Transform** : AWS::Serverless-2016-10-31 is **required** for AWS SAM template files, **Transform** is what identifies it as AWS SAM template instead of CloudFormation template
  - Others include , Globals, Resources, and Parameters
- **sam package, sam deploy** serverless application
- **CloudFormation package , then CloudFormation deploy**
- **SAM** is extension to CloudFormation, and is more simpler.
- Deployment packages are stored in S3
- **Rolls back** the deployment if CloudWatch alarms are triggered
- Deployment Preference Type
  - **Canary** – 2 Increments ( ex: 10% and 90% or 35% and 65%)
  - **Linear** - equal increments , equal min
  - **All-at-once –** All at once.
- **AWS::Serverless::Application**  : Define a **Nested application**
- **AWS::Serverless::Function** : C **reating a Lambda function**
- **AWS::Serverless::LayerVersion** : C **reating a Lambda layers**
- **AWS::Serverless::Api** : Describes an **API Gateway resource**
- **AWS::Serverless::HttpApi** : Creates an Amazon **API Gateway HTTP API**
- **AWS::Serverless::StateMachine** : Creates AWS **Step Functions state machine**
- [https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-specification-resources-and-properties.html](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-specification-resources-and-properties.html)
- SAM has its own cli (different from aws cli)
  - _sam init_ : Initialize a serverless application with SAM template
  - _sam validate_ : validate SAM template
  - _sam build_ : Builds serverless application
  - _ **sam package** _: **Bundle you application code and its dependencies** into deployment package
  - _ **sam deploy** _ **: Deploy SAM application to AWS**
  - [**https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-command-reference.html**](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-command-reference.html)

## ECS - Elastic Container Service

- **ECS tasks can also be assigned with IAM ROLES** just like IAM Role or EC2 instances
- **Task definitions**
  - Container information for your application
    - **Docker image to use** with each container , Docker networking mode
    - **CPU and memory** to use with each task
    - The **IAM role** that your tasks should use
    - The **launch type** to use (EC2 or Fargate)
    - The **logging** configuration,
    - what to do in case of failures
    - The command the container should run when it is started
    - Any data volumes that should be used with the containers in the task

- **ECS Task placement**
  - **Task placement strategies**
    - **Algorithm for selecting instances** for task placement or tasks for termination.
    - _ **binpack** _
      - **minimizes the number of instances** in use
      - Place tasks based on the **least available amount of CPU or memory**
    - _ **random** _
      - **randomly** place the tasks
      - requires the **LEAST** amount of configuration **(EASIEST)**
      - follow the **constraints** that you specified both **implicitly or explicitly**
      - it makes sure that tasks are **scheduled** on **instances**** with enough resources to run them**
    - _ **spread** _
      - Place tasks **evenly based on the specified value**
        - Host (_instanceId_)
        - Availability Zone ( _attributes:ecs.availabiltiy-zone__)_
  - **Task placement constraints**
    - **Rule** that is considered during task placement.
    - Ex: place tasks **based on Availability Zone** or **instance type**
    - _distinctInstance_
      - _Place each task on a different container instance_
    - _memberOf_
      - _based on expression_
    - **Task groups:** set of related tasks
  - **Cluster query language**
    - **Expressions** that enable you to **group objects**
    - you can **group container instances by Availability Zone, Instance type, or custom metadata**
  - **Task placement** constraints **place** the tasks, but _ **Cluster query language** _ is the one which provides you with _ **expression** _ **to group the task**
  - Task placement strategies and constraints are usually used together
  - Task placement strategies and constraints are **NOT supported for tasks using the Fargate launch type**.
  - **Fargate tasks are spread across Availability Zones**
  - [https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-placement-strategies.html](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-placement-strategies.html)
- **ECS Deployment types**
  - **Rolling update**
    - Amazon ECS service scheduler replaces the currently running tasks with new tasks
  - **Blue/Green deployment with CodeDeploy**
    - Controlled by CodeDeploy
    - enables you to verify a new deployment of a service before sending production traffic to it
    - **Canary** : Traﬃc is shifted in **two increments**.
    - **Linear** : Traﬃc is shifted in **equal increments** with an **equal number of minutes between each increment**
    - **All-at-once:**** All traﬃc** is shifted from the original task set to the updated task set all at once.
  - **External Deployment**
    - third-party deployment
- **ECR**
  - Fully Managed Docker container registry provided by AWS
  - Alternative to Docker Hub

## Elastic Beanstalk

- AWS Elastic Beanstalk **automatically** handles tasks such as **provisioning of the resources, load balancing, auto-scaling, monitoring** , and **placing the containers across the cluster**
- **You**** still ****retain full control over AWS resources** (EC2, ELB, RDS, ASG)
- Best **suited for simple web application** and **NOT micro services architecture (**ECS is best suited**)**
- **Source bundle** :
  - Consist of a **single ZIP file or WAR file** (you can include multiple WAR files inside your ZIP file)
  - **Not exceed 512 MB**
  - **Not include a parent folder or top-level directory** (subdirectories are fine)
- **cron.yaml** : is used when you deploy a **worker application** that processes **periodic background tasks**
- **.ebextensions**
  - Configuration files with a **.config file extension are placed in a folder named .ebextensions**
- Deployment Policies
  - **All at once** :
    - **Fastest** deployment
    - **No additional Cost**
    - **Complete outage , Needs downtime, all instances are taken down**
    - Deploys the new version to **all instances simultaneously**
  - **Rolling:**
    - Update a few instances at a time (batch), and then move onto the next batch once the first batch is healthy
    - Each batch is taken out of service during the deployment phase
    - **No additional Cost**
    - Reduced capacity of instance which impacts application performance
  - **Rolling with Additional batch**
    - Additional new batch/set of instance are created, version deployed in stages
  - **Immutable** :
    - Total deployment of **new instances**
    - **Higher cost** , because you bring up equal number of new instances
  - [https://blog.shikisoft.com/which\_elastic\_beanstalk\_deployment\_should\_you\_use/](https://blog.shikisoft.com/which_elastic_beanstalk_deployment_should_you_use/)
  - [https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html#deployments-scenarios](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html#deployments-scenarios)

![](RackMultipart20210630-4-ccnkca_html_752a966f701e0548.png)

[https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html)

- **Worker environment**
  - when **operations or workflows that take a long time to complete**
  - Long-running task such as processing images or videos, sending email, or generating a ZIP archive use worker environment in EB
  - Can be clubbed with SQS

- **AWS CLI deployment :** Create new version and deploy

1. _ **eb appversion** _
2. _ **eb deploy** _

## Lambda

- To **increase performance keep connect or close statements outside of the handler** , Place the connection in the global space or have login in program to check db connectivity before making connections
- Lambda **needs permission to be granted to write to CloudWatch logs** , if no permission then nothing will be written to CloudWatch logs
- If **no logs are being written to CloudWatch logs** lambda **doesn&#39;t not have permission to write logs**
  - Most important ones are below 2
    - _ **AWSLambdaBasicExecutionRole** _ : **write logs to CloudWatch**
    - _ **AWSXRayDaemonWriteAccess** _: **upload trace data to X-Ray**
  - Others include
    - _AWSLambdaSQSQueueExecutionRole_: read a message from SQS.
    - _AWSLambdaVPCAccessExecutionRole_: to manage elastic network interfaces to connect your function to VPC
    - _CloudWatchLambdaInsightsExecutionRolePolicy_: To write runtime metrics to CloudWatch Lambda Insights.
    - _AWSLambdaDynamoDBExecutionRole_: Read records from DynamoDB stream.
    - _AWSLambdaKinesisExecutionRole_: events from Kinesis data stream or consumer.
    - _AWSLambdaMQExecutionRole_: read records from an Amazon MQ broker.
    - _AWSLambdaMSKExecutionRole_: Permission to read records from Amazon MSK cluster.
- You **increase memory** (128MB to 10GB in 1MB incr) **to increase CPU, (** this increases cost)
- **Aliases**
  - Pointer to a specific **Lambda function version**
- **Layers**
  - A layer is just a ZIP archive that contains libraries, a custom runtime, or other dependencies
  - used to pull in additional code and content in the form of layers
- **Environment variables**
  - Enable you to dynamically pass settings to your function code and libraries, without making changes to your code
  - Ex: Lambda function which will run in various environments such **as DEV, TEST, UAT, and PROD** , and call different API for different environments
- **Synchronous invocation**
  - **Wait for the function to process the event and return a response** (StatusCode: 200)
  - Use cases : API Gateway, CloudFront
- **Asynchronous invocation**
  - Lambda **queues the event for processing** and **returns a response immediately** and without additional information **(StatusCode: 202)**
  - use the _ **invoke** _command with **--invocation-type** parameter to **Event**
  - Use cases: S3 , SNS
- HTTP **Status Code: 429 ( or 502)**: _ **TooManyRequestsException** _ ,
  - **Exceeded allowed concurrency** , **throttling** occurs with 429
  - The request **throughput limit was exceeded**  **for Synchronous invocations**.
  - Asynchronous invocation  retry, then to DLQ (Dead letter Queue)
- HTTP **Status Code 504** (5XX)
  - INTEGRATION\_FAILURE, INTEGRATION\_TIMEOUT
  - INTEGRATION\_TIMEOUT range is **50 milliseconds to 29 seconds**
- _ **InvalidParameterValueException** _
  - one of the parameters in the request is invalid, ex: if you provided an IAM role in the _CreateFunction_ API which AWS Lambda is **unable to assume IAM role**
  - Unzipped package is bigger than allotted size
- _ **CodeStorageExceededException** _
  - exceeded your maximum total code size per account
- _ **ResourceConflictException** _
  - resource already exists
- _ **ServiceException** _
  - AWS Lambda service encountered an internal error
- **Lambda Limits**

| **Resource** | **Quota** |
| --- | --- |
| Function memory allocation | 128 MB to 10,240 MB (10GB) , in 1-MB increments. |
| Function timeout (Execution Time) | 900 seconds ( **15 minutes** ) (default is 3sec) |
| Function environment variables | 4 KB |
| Function resource-based policy | 20 KB |
| Function layers | 5 layers |
| Function burst concurrency | 500 - **3000** (varies per Region) |
| Deployment package (.zip file archive) size | **50 MB** (zipped, for direct upload)
**250 MB** (unzipped, including layers)
 3 MB (console editor) |
| Container image code package size | 10 GB |

- You **CANNOT**** configure an event-source mapping between ALB and a Lambda function ,** instead use ALB target type as lambda
- To access RDS instance that is in a **private subnet** , configure _ **Lambda function to connect to your VPC** _ ** **
- **Session data** and Lambda
  - AWS Lambda is a stateless compute service and so you cannot store session data in AWS Lambda itself
  - You can use **DynamoDB or AWS S3** with lambda to store some session data (user data)

- **Concurrency** : is the number of requests that your function is serving at any given time
  - **Push-based** event sources ( **S3 , API Gateway** )
    - **Concurrency = number of requests \* number of executions per sec.**
    - Ex: if we have 20 requests and each requests takes 20 sec to execute then concurrency = 20\*20=400
  - **Poll-based** event sources ( **Kinesis , DynamoDB** )
    - **Concurrency = Number of active shards**
    - Ex: If Kinesis Data Streams stream which has 100 active shards. The Lambda function takes 10 seconds to process the data and the stream is receiving 50 new items per second, concurrency = 100 (number of active shards)
- **Exponential backoff**
  - Helps to Lambda to retry by Increasing the wait time between retries for consecutive error responses
- **Lambda Best Practices**
  - **Separate the Lambda handler (entry point) from your core logic**.
  - **Take advantage of Execution Context** reuse to improve the performance of your function
  - Use AWS Lambda **Environment Variables to pass operational parameters** to your function.
  - **Avoid using recursive code**
  - Minimize your deployment **package size to its runtime necessities**.
  - Control the dependencies in your function&#39;s deployment package.
  - Reduce the time it takes Lambda to unpack deployment packages
  - Minimize the complexity of your dependencies
- **Cold Starts**
  - When the first invocation is made to **lambda it takes time (400millisec) to spin up** that is called cold start problem (delay) , to solve this uses **preserved concurrency** (keep lambda instance up and running) but it **costs more**
- **Lambda inside VPC**
  - Lambda creates Elastic Network Interface
  - Uses **AWSLambdaVPCAccessExecutionRole**
    - ec2:CreateNetworkInterface
    - ec2:DescribeNetworkInterfaces
    - ec2:DeleteNetworkInterface
  - **NAT Gateway** :
    - To Provide internet access to lambda function inside VPC, private subnet should have route to NAT Gateway
- **Lambda CloudFormation – Deployments**
  - AWS::Lambda::Function

## Step Functions

- Series of steps that make up a workflow
- Simple workflows
- **Combine Lambda** , or **branching**** Lambda **or** parallel **processing or** Error handling**

![](RackMultipart20210630-4-ccnkca_html_f95860abd88605c8.png) ![](RackMultipart20210630-4-ccnkca_html_b1beb7ee27eb3d43.png) ![](RackMultipart20210630-4-ccnkca_html_9dce2b0f9ec11d91.png) ![](RackMultipart20210630-4-ccnkca_html_62c40b64ebf6b104.png)

- **State machine** (workflow) and tasks

## SFW – Simple Workflow Services

- Complex workflows

## API Gateway

- &quot; **Front Door&quot;** to your APIs
- **Fully Managed** , **create** , **publish** , **maintain** , **monitor** , and **secure**** APIs at any scale**
- **HTTP 504 status code :**
  - Integration request takes longer than your API Gateway REST API maximum integration timeout parameter
  - Underlying Lambda function has been running for **more than 29 seconds causing the API Gateway request to time out**
- **HTTP 429:**
  - **Throttling errors , too many request** coming in from users
  - Can be avoided by **setting request limit** for steady state and burst (max num of concurrent requests)
- **Stage Variable**
  - Configure HTTP endpoint for your stages talk to **(dev, prod, test)**
  - Stage variables are **used along with lambda alias**
  - Pass configuration parameters (changing values) to a Lambda from API Gateway
  - API Gateway ==== pass parameters ( use **stage variable** )====\&gt; Lambda
  - Ex: You may want to **re-use the same Lambda function** for multiple stages in your API, **but the function should read data from a different Amazon DynamoDB table**** depending on which stage is being called**
- **API Integration**
  - You choose an API integration type **according to the types of integration endpoint**
  - **Lambda Function**
    - LAMBDA\_PROXY (AWS\_PROXY)
      - Simple, If your API **does not require content encoding or caching** ,
      - flexible, versatile, and streamlined integration setup
    - LAMBDA\_CUSTOM (non-proxy)
      - **lambda\_proxy +** how the incoming **request data is mapped** to the integration + resulting integration **response data is mapped** to the method response
  - **HTTP (**more **simpler API Integration)**
    - **Newer** , **cheaper** , **low latency** and **simple** to set up
    - HTTP PROXY
    - HTTP (HTTP Custom)
  - Mock : used for testing/simulations no real backend
  - **AWS Service (only non-proxy)**
  - VPC Link
- **API Gateway CloudWatch metrics**
  - By default, API Gateway metric data is automatically sent to CloudWatch in one-minute periods
  - **Latency**  : measure the overall **responsiveness of your API calls**
  - **IntegrationLatency :** measure the **responsiveness of the backend**
  - **CacheHitCount**  : number of requests **served from the API cache** in a given period
  - **CacheMissCount**  : number of requests **served from the backend** in a given period
- **Mapping Templates :**
  - Script written in VTL (Velocity Template Language)
  - Convert from json to XML (one form to another)
- **Lambda authorizer :**
  - API gateway feature that uses a Lambda function to control access to your API
  - takes the caller&#39;s identity as input and returns an IAM policy as output
  - **Token-based** (TOKEN authorizer ) Lambda authorizer
    - Caller&#39;s identity in a bearer token, such as a **JSON Web Token (JWT)** or an **OAuth token.**
  - **Request parameter-based** (REQUEST authorizer) Lambda authorizer
    - Caller&#39;s identity in a combination of **headers, query string parameters, stageVariables** , and **$context variables**

## SQS

- **Standard queues**
  - **at-least-once** message delivery
  - **duplicate messages** can be delivered
  - **best effort** ordering
  - **Better throughput** then FIFO
- **FIFO queues**
  - _ **exactly-once** _ _processing_ _ **,** _
  - _ **No Duplicate** _ _messages_
  - _ **FIFO** _ _Order_
  - _ **Low throughput** _
- _Deduplication of messages can be enabled by_
  - Enable **content-based deduplication**
  - Explicitly provide the **message deduplication ID**
- **SQS APIs**
  - **create\_queue** : Creates a new standard or FIFO queue
  - _ **delete\_message** _: Deletes the specified message from the specified queue
  - _ **delete\_message\_batch** _: Deletes up to 10 messages from the specified queue
  - _ **delete\_queue** __ **:** _ Deletes the queue
  - _ **purge\_queue** _ **:** Deletes the messages
  - _ **list\_queues** _ **:** list of your queues in the current region
  - _ **receive\_message** _ **:** Retrieves one or more messages (up to 10)
  - _ **send\_message** _ **:** Delivers a message
  - _ **send\_message\_batch** _ **:** Delivers up to 10 messages
  - _ **change\_message\_visibility:** _ Changes the visibility timeout message
  - _ **change\_message\_visibility\_batch:** _ Changes the visibility timeout multiple messages
  - [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sqs.html#id56](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sqs.html#id56)

- AdjustReceiveMessage  parameter if consumer is not able to process incoming messages , ex: setReceiveMessage to 10 to retrieve 10 messages at a time
- **Short Polling :**
  - Default SQS behavior , Get msg  send msg (returns immediately) ,
  - Responds even if no msgs in queue. Empty responses possibility is more
  - WaitTimeSeconds =0 or [ReceiveMessageWaitTimeSeconds](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SetQueueAttributes.html) = 0
- **Long Polling :**
  - WaitTimeSeconds = 20  Wait till some msg arrive (orReceiveMessageWaitTimeSeconds reaches) in queue then send msg
  - Responds only when at least one msg is in queue , sends an empty response only if the polling wait time expires (max 20sec)
  - **Use case (recommendation)**
    - **Reduce Cost:**
      - Reduce the num. of API calls you need to make
      - **Reduce number of empty responses**** and false empty responses**
    - If the **new messages** that are being added to the SQS queue **arrive less frequently**
    - **Faster updates:** Receive message **as soon as they arrive**

- **VisibilityTimeout** :
  - Period of time SQS **prevents other consumers from receiving** and processing the message which was already processed, (avoid duplicate processing)
  - **Increase VisibilityTimeout (****ChangeMessageVisibility**)**to reduce duplicate msgs being processed**
  - Default is 30sec, min=0sec , max=12hrs
  - Used when your application takes a long time to process and delete ( **DeleteMessage** ) a message from the SQS queue
- **Delay Queues:**
  - Postpone the **delivery of new messages to a queue** for a number of seconds
  - Your consumer application needs additional time to process messages
- **VisibilityTimeout** Vs. **Delay Queues**
  - **VisibilityTimeout :** message is consumed once and then timeout is set for other consumers
  - **Delay Queues :** Message itself it delayed to be processed by any consumer for first time itself

- **Dead Letter Queue:**
  - Special queue for messages that can&#39;t be processed (consumed) successfully
  - Helps in debugging your application or messaging system
- **MessageGroupIds**
  - To **process** messages **one by one, in a strict order****  **(in particular message group)
- **Auto Scaling SQS**
  - **Target tracking scaling policy (**ApproximateNumberOfMessages)

## Kinesis

- Handle Streaming data

## Kinesis Streams

- **real-time**** data streaming** service (DynamoDB does not provide real-time injection to DB)
- massively scalable and durable can continuously capture gigabytes of data per second from hundreds of thousands of sources
- **website clickstreams** , database event streams, **financial transactions** , **social media feeds** , IT logs, and **location-tracking events** , **player-game interactions** , **stats updates**

- Single KCL worker can handle any number of shards
- Number of KCL workers = \&lt; Num. of shards
- When question is about **maximum KCL workers then = num. of shards** (1:1 ratio)
- by **default** data records are **only accessible for 24 hours** , can be changed to max of 8760 hrs (365days)
- **Resharding** : enable your stream to adapt to changes in the rate of data flow
  - **Split shards** : To **increase the capacity** of your stream, **increases cost** also.
  - **Merge shards** : To **decrease the capacity** of your stream, **reduce cost**
  - **Hot** shards: shards receiving **more data**
  - **Cold** shards: shards receiving **less data**

## RDS

- **OLTP , Transitional applications, complex queries**
- RDS is **NOT a key/value store**
- You can enable **Slow query logs** to troubleshoot SQL logs within RDS
- **Transparent Data Encryption** (TDE)
  - Automatically **encrypt data** before it is written to storage, and automatically **decrypt data** when the data is read from storage.
- **Multi-AZ deployments** are **synchronous** , it **improves availability** and **NOT scalability** (use read replicas for scalability)
- **Read Replicas**
  - Support **read-heavy database** workloads
  - **Enabling autobackp is mandatory** before creating read replicas
  - Apps can connect directly to read replica for read operation
  - Only used for reads
  - **Improve scalability**
  - Can be in **same or different AZ or region**
  - Are not deleted when main db is deleted, you need to delete by yourself

## DynamoDB

- [https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/](https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/)
- No need to provision DB, only create RCU and WCU
- **Partition key** : Unique identifier, A simple primary key,
- **Composite primary key** _=_ **Partition key and sort key**
- Recommendations for partition keys
- **Use high-cardinality attributes**. These are attributes that have distinct values for each item,
  - _e-mailid, employee\_no, customerid, sessionid, orderid_, and so on.
- Use composite attributes. Try to combine more than one attribute to form a unique key, if that meets your access pattern
- Can **save session state** of the users (but it has **higher latency then Elasticache** )
- Session state with low latency then go for Elasticache and not DynamoDB
- RCU WCU – how to calculate ?
- Read Capacity Unit – RCU
  - **4KB** item , **1RCU** can perform **1 strongly consistent** read/sec
  - **4KB** item, **1RCU** can perform **2 eventually consistent** read/sec
  - _Transactional_ reads: 4KB item , 2RCU can perform 1 read/sec
  - Ex: **4KB item and 10RCU** , can perform 10\*1= **10 strong consistent** and 10\*2= **20 eventual consistent reads** and 5 _Transactional_ reads
- Write Capacity Unit – WCU
  - **1KB** item , **1WCU** can perform **1 standard consistent** write/sec
  - _Transactional_ write: 1KB item , 2WCU can perform 1 write/sec
  - Ex: **1KB item and 10WCU ,** Can perform 10 standard consistent writes/sec

- DynamoDB **Streams** :
  - To keep **records of changes to items in a DynamoDB** table
  - All data in DynamoDB Streams is subject to a **24 hour lifetime, any data beyond 24hrs gets deleted from streams**
  - decrease the interval of running your lambda function to 24 hours, in case if some data is missing on your consumer (S3)
  - DynamoDB Streams can also be used as trigger which **can automatically trigger notification to lambda to send mails, notifications** when data change occurs in table (no need of CloudWatch integration)
  - _ **StreamViewType** _ are:
    - _ **KEYS\_ONLY** _ - Only the key attributes of the modified item are written to the stream.
    - _ **NEW\_IMAGE** _ - The entire item, as it appears **after it was modified** , is written to the stream.
    - _ **OLD\_IMAGE** _ - The entire item, as it appeared **before it was modified** , is written to the stream.
    - _ **NEW\_AND\_OLD\_IMAGES** _ - **Both the new and the old item** images of the item are written to the stream.
- **Scan and Query**
  - Scan queries entire table, use query instead of scans
  - For **Better performance** of (or **reduce impact** on) provisioned throughput, or reduce impact on
    - **Use query**
    - **Reduce page size**
    - **Isolate scan operations**
  - **Faster scan** results, but will place more burden on the table&#39;s provisioned throughput ( to **reduce the burden**** apply rate limiting**)
    - **parallel Scan (better used in below scenarios)**
      - The table size is 20 GB or larger
      - The table&#39;s provisioned read throughput is not being fully used.
      - Sequential Scan operations are too slow.
  - _Sparse index : Queries over a small subsection of a table_

- **DynamoDB Batch Operation**
  - Helpful when there are too many transaction (read/write) happening on your DynamoDB causing network overhead or application performance degradation
  - Use   **BatchGetItem**  and  **BatchWriteItem**  operations to reduce number of network trips
  - _Batch Operations API_ _ **GET, PUT, and DELETE** _ _operations_
- **DynamoDB transactional read**
  - **atomicity, consistency, isolation, and durability (ACID)** in DynamoDB,
  - Maintain **data correctness** in your applications easily
  - require **coordinated inserts, deletes, or updates to multiple items**
- **Projection expression** : returns **attributes ( Ref. table headers)**
- **Filter expressions** : return **items (actual values )** for your query
  - Attributes
-   **Cache-Control: max-age=0**
  - Invalidate cache and pull the data from DynamoDB directly
- **Time To Live (TTL)**
  - Enable TTL to **automatically clear session data or stale data in DynamoDB**
  - To clear Ex: session data, event logs, usage patterns, userlogins
- **Global secondary index**
  - Perform fast queries on specific columns in a table
  - An index with a **partition key and a sort key that can be different** from those on the base table
  - Supports **ONLY eventual consistency**
  - To **query over the entire table** , across all partitions.
  - You can add a GSI to an already existing table
  - Has **its own RCU and WCU**
  - **Speed up queries**** on ****non-key attributes use a GSI**
  - **Improve query performance and reduce costs**
  - **WCU of GSI =\&gt; WCU of base table**
- **Local secondary index**
  - Perform fast queries on specific columns in a table
  - An index that has the **same partition key as the base table, but a different sort key**
  - Support **both**** eventually consistent and strongly consistent reads**
  - To **query over a single partition** of the table
  - LSI are **created at the same time you create a table**. You cannot add/delete a LSI to an existing table
  - **Uses parent RCU and WCU**

| **Global Secondary Index** | **Local Secondary Index** |
| --- | --- |
| fast queries on specific columns in a table | fast queries on specific columns in a table |
| **partition key and a sort key that can be different** from Base table | **same partition key as the base table, but a different sort key** |
| ONLY **eventual consistency** | **eventually and strongly consistency** |
| **query over the entire table** | query over a **single partition** |
| **Can add a GSI to an already existing table** | Created at the same time you create a table. You **cannot add/delete a LSI to an existing table** |
| Has **its own RCU and WCU** | Uses **parent RCU and WCU** |
| Speed up/retrieve queries on **non-key attributes** |
 |

- **DynamoDB write operations:**
  - To create, update, or delete an item in a DynamoDB table, use one of the following operations:
    - _PutItem_
    - _UpdateItem_
    - _DeleteItem_
  - To return the number of write capacity units consumed by any of these operations, set the _ **ReturnConsumedCapacity** _ parameter to one of the following
    - _ **TOTAL** _: returns the **total number of WCU consumed**.
    - _ **INDEXES** _: returns the **total number of WCU consumed** , with **subtotals for the table** and any **secondary indexes that were affected by the operation**.
    - _NONE_: no write capacity details are returned. (This is the default.)

- **Automic\_counters**
  - to keep track of the **number of visitors to a website**
  - count may not be always exact may be slight over counting or undercounting, hence cannot be used for banking application or similar where count needs to be accurate.
  - use UpdateItem for incrementing
  - for cases like banking application use _conditional updates_

- **DynamoDB Accelerator (DAX)**
  - In-memory caching system used inform of DynamoDB
  - **performance improvement**** from milliseconds to microseconds** for DynamoDB

- _ **ProvisionedThroughputExceeded** _ you can avoid this error by
  - Reduce page size
  - Isolate scan operations
  - **Error Retries and Exponential Backoff**

## ElastiCache

- In-memory caching system , memcached and redis
- When to choose what?

| **Memcached** | **Redis** |
| --- | --- |
| **simplest model** possible | **Snapshots**  **, automatic failover, backup/restore** |
| **Multi-threaded** : Run large nodes with multiple cores or threads | **Replication** |
| ability to **scale in and out** | **Geospatial support** |
| **adding and removing nodes as demand** | Complex data structures: sorted sets and lists, Transactions |
| **cache objects** | Pub/Sub |
|   | Lua scripting |
|
 | Highly available then Memcached |

- **Caching strategies (Memcached)**
  - Add **Time to live (TTL) to avoid stale data or cache churn**

| **Lazy loading** | **Write-through** | **Adding TTL** |
| --- | --- | --- |
| **Load cache only when required** **(Cache Miss occurs)** | **Adds/updates data in the cache whenever data is written to the database**. | Integer value in sec that specifies until the key expires |
| **Only requested data is cached** | all the data is cached |   |
| lazy loading **avoids filling up the cache with data that isn&#39;t requested** | **Cache churn** : Most data is never read, which a waste of resources is. | **Add TTL to avoid cache churn** |
| Node failures aren&#39;t fatal for your application | Missing data occurs if node fails |   |
| **Stale data** | Data in the cache is never stale | **Add TTL to avoid stale data** |

## Cognito

- Must be **accessible from**  **mobile** , **desktops** , and **tablets** , and must remember user preferences across platforms
- Supports **unauthenticated access** to users
- provide the sign-up and sign-in functionality to end users with no AWS access

| **Cognito User Pool** | **Cognito Identity Pool (Federated Identities)** |
| --- | --- |
| **Authentication** | **Authorization** |
| **sign-up and sign-in webpages** for your app | Generate **temporary AWS credentials for unauthenticated users**.
 Guest access or Anonymous access |
| Access and manage user data. | Give your users **access to AWS resources , like S3 , DynamoDB** |
| Use a custom authentication flow |   |
| Track user device, location, and IP address |   |
| adapt to sign-in requests of different risk levels |   |

- You can add **multi-factor authentication (MFA)** to a user pool as second authentication method (SMS text messages, or time-based one-time (TOTP))
- [https://aws.amazon.com/premiumsupport/knowledge-center/cognito-user-pools-identity-pools/](https://aws.amazon.com/premiumsupport/knowledge-center/cognito-user-pools-identity-pools/)
- **Cognito Sync**
  - **Synchronize the user profile data** across mobile devices

## AWS AppSync

- Similar to Cognito sync but it also allows **multiple users to synchronize and collaborate shared data in real time**

## CodeCommit

- Access to CodeCommit
  - HTTPS Git Credentials (Generate username and pwd)
  - SSH Keys (Upload public ssh keys)

## CodeBuild

- Build spec file can be only YAML based file **buildspec.yml** (Cannot have json based build spec file) (for codedeploy you can have either appspec.yml or appspec.json)

## CodePipeline

- Create Pipelines with multiple stages
  - Build, Test, Release, Deploy

## CodeDeploy

- Can deploy software packages using an archive that has been uploaded to an Amazon S3 bucket
- Can deploy application content **from S3, GitHub, or Bitbucket**
- Can be used for deployment activities **on**** EC2, On-Prem, Lambda or ECS**
- **CodeDeploy agent**** must be installed (EC2 and On-prem ONLY) first** to use CodeDeploy
- CodeDeploy **agent is not required for Amazon ECS or AWS Lambda**
- CodeDeploy agent communicates outbound using **HTTPS over port 443**

- Deployment types
  - **In-place deployment** : on each instance in the deployment group

    - Applicable to **EC2 and On-Prem ONLY** , CANNOT be used on ~~ECS or Lambda~~

1. Application stopped
2. Latest app is installed
3. New version of the app is started and validated
- **Blue/green deployment** :

    - Applicable to **EC2 , ECS, and Lambda,** CANNOT be used on ~~ON-Prem~~

    - EC2
      - Instances in original deployment group gets **replaced** with new group of EC2 instances
    - ECS
      - **Traffic is shifted** from original tasks set to new task set
      - Load balancer are used to reroute traffic
    - Lambda
      - **Traffic is shifted** from original lambda to newer version of lambda

- When **roll back** happens CodeDeploy first updates the **failed EC2 instances**

## CodeStar

- **Quickly**** setup version control **,** build and deployment **with** project dashboard** – answer is CodeStar
- Provides the tools you need to quickly develop, build, and deploy applications on AWS
- Provides project **dashboard** to **centrally monitor application activity**** and manage day-to-day development tasks**
- Integrates with Atlassian JIRA, a third-party issue tracking and project management tool

## AWS Secrets Manager

- **Store secrets/credentials** and **automatically rotate them**
  - Ex: database credentials are encrypted and periodically rotated
- Integration for Amazon RDS, Amazon Redshift, and Amazon DocumentDB

## AWS SSM Parameter

- Can **store secrets** (SecureString) but **NO automatic rotation** , you/application have to rotate if needed.

## AWS KMS keys

- **Multi-tenant** Key Management Service
- Region Specific
- **Symmetric** : **single key** for both encryption and decryption
- **Asymmetric** : 2keys , **Public and Private keys** for Encryption and Decryption
- **Envelope Encryption** :
  - Encrypting plaintext data with a data key, and then encrypting the data key under another key
  - **master\_key( data\_key[Plaintext])** or
  - **master\_key{ data\_key (data\_key [Plaintext] ) }** and so on
  - This top-level plaintext key encryption key is known as the _ **master key** _
  - KMS helps you to encrypt this master key (plain text) and it&#39;s called customer master keys **KMS CMKs**
- **Request quotas**
  - 5,500 to 30000 per sec
  - **ThrottlingException :** When you exceed the allotted quotas
    - you could **lower your request rate**
    - **Retry**** with ****exponential backoff**
- **CloudTrail:** Can be used to **track KMS CMK&#39;s**

## CloudHSM

- **Dedicated Single-tenant** HSM
- You need **hardware security module** (HSM) appliance to store your encrypted keys

- **FIPS 140-2 compliance**
- Integration with applications using **PKCS#11, Java JCE, or Microsoft CNG interfaces**.
- High-performance in- **VPC cryptographic acceleration** (bulk crypto)

## Amazon Guard​Duty

- **Threat detection** , continuously monitors for **malicious activity and unauthorized** behavior and protect your AWS accounts and workloads

## CloudWatch

- **Namespaces :**
  - Metrics in different namespaces are isolated from each other
  - graphical representation of the key performance metrics for each EC2 instance
- **Metrics**
  - **Default** metric (EC2) time period is **5min,** if **detailed enabled** then **every 1min** and has **additional cost.**
  - CloudWatch can track custom metrics such as **memory, swap, and disk space** utilization but it&#39;s **not available by default**. You need to **install CloudWatch agent** in your EC2 instances
- **Dimensions**
- **Resolution**
- **Statistics**
- **Percentiles**
- **Alarms**
- **CloudWatch Events**
  - Respond to **state changes** in your AWS resources ( useful for triggering lambda func)
- **HTTP 400 ThrottlingException** for PutMetricData API calls in CloudWatch
  - **Retry your call with exponential backoff and jitter**.
  - Distribute your API calls evenly over time rather than making several API calls in a short time span
  - Combine as many metrics as possible into a single API call.
  - [https://aws.amazon.com/premiumsupport/knowledge-center/cloudwatch-400-error-throttling/](https://aws.amazon.com/premiumsupport/knowledge-center/cloudwatch-400-error-throttling/)

## CloudTrail

- **Auditing** , Used for logging , continuously monitor, and retain account activity related to actions across your AWS infrastructure , ex : **API activity related to creating, modifying or deleting AWS resources**
- **Event history of your AWS account activity** (console, or cli or SDK) , **Who , what, when ?**
- **Multi Region Trail :** One trail for all AWS regions
- **Single Region Trail:** only events from one region

## AWS X-Ray

- **Tracing application activity** for **performance** of applications and operational statistics
- Analyze user requests as they **travel through your Amazon API Gateway APIs** to the underlying services
- To collect logs from EC2, **install the X-Ray daemon by using a user data script**.
- Useful for **governance, compliance, operational auditing,** and **risk auditing**
- **Segments** : The **data** , like the hostname, alias , IP, start and end times, subsegments , status
- **Subsegments** : **more granular details of segments.**
- **Filter expressions** : find **traces related to specific paths or users**.
- **Annotations** : key-value pair, **searchable** , **Indexed** , and **used along with Filter expressions**
- **Metadata** : key value pair, **NOT indexed** , **NOT searchable** , used for record data you want to store in the trace but don&#39;t need to use for searching traces
- Listens for traffic on **UDP port 2000**
- **Errors** : Client errors **(400 series errors)**
- **Faults** : Server faults ( **500 series errors** )
- **Throttle** : Throttling errors ( **429 Too Many Requests** )

- [https://docs.aws.amazon.com/xray/latest/devguide/xray-concepts.html](https://docs.aws.amazon.com/xray/latest/devguide/xray-concepts.html)
- AWS IAM is used to grant X-Ray permissions to users and compute resources in your account
  - **AWSXrayReadOnlyAccess :** Access to X-Ray Console,view service maps and segments
  - **AWSXRayDaemonWriteAccess :** upload traces, and some read permissions to support the use of sampling rules
  - **AWSXrayFullAccess: Encryption key** settings and **sampling rules**
- **DOES NOT** track ~~**memory, swap, and disk space**~~ use CloudWatch for such custom metrics

## AWS Config

- **Auditing, Inventory of AWS**  **resources**
- **Resource**  **history and change tracking** – how resource was configured,
- **Governance**

## AWS Shield

- **Avoid DDoS Attacks**

## AWS WAF

- **Protect from SQL injection** , **Cross-site scripting**
