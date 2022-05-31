# CloudWatch

CloudWatch is used for monitoring.
### CW Could monitor
- Compute: Ec2, ASG, ELB, Route53, Lambda
- Storage: EBS, Storage Gateway, CloudFront
- Database: DDB, RDS, RS, EMR
- Other: SQS, SNS, API gateway, charges. 

### Why is monitoring important?
- To deploy applications
    - Safely
    - Automatically
    - Using Infrastructure as Code
    - Leveraging AWS components
- Because applications are deployed, and users don’t care what services we've used
- Users only care that the application is working!
    - Application latency: will it increase over time?
    - Application outages: customer experience should not be degraded
    - Users contacting the IT department or complaining is not a good outcome
    - Troubleshooting and remediation
- Internal monitoring:
    - Can we prevent issues before they happen?
    - Performance and Cost
    - Trends (scaling patterns)
    - Learning and Improvement

### Monitoring in AWS
- AWS CloudWatch:
    - All about performance and metrics, logs, and alarms of AWS resources. 
    - Metrics: Collect and track key metrics
    - Logs: Collect, monitor, analyze and store log files
    - Events: Send notifications when certain events happen in your AWS • Alarms: React in real-time to metrics / events
- AWS X-Ray:
    - Troubleshooting application performance and errors
    - Distributed tracing of microservices
- AWS CloudTrail (Audit Trails):
    - Internal monitoring of API calls being made
    - Audit changes (create, modify, delete, failed login) to AWS Resources by your users
    - Record last 90 days activity
    - Delivers log files containing API calls to an S3 objects; can be integrated with CW logs. 

### CloudWatch Agent
- By installing the CW agent on your EC2 instances, you can collect OS metrics and send them to CW. 
- Demo 
  - Launch an EC2 instance: attach an IAM role with permissions to send CW agent metrics to CW
     - Role: CloudWatchAgentServerPolicy - permission to send metrics to CW
  - Install CW Agent: Configure the agent to send OS metrics and logs to CW
     - run cmd to install, configure, configuration validations, restart
  - View Metrics in CW: we should be able to see the EC2 default metrics and well as the metrics and logs send by the CW Agent. 
     - EC2 -> metric per instance -> CW Agent -> CPU Utilization

### CloudWatch Metrics
- CloudWatch provides metrics for every services in AWS
- **Metric** is a variable to monitor (CPUUtilization, NetworkIn...); Time-ordered sequence of values or data points
- Metrics belong to **namespaces** ; namespace is a container of metrics. 
   - Create your own namespace to publish custom metric data
- **Dimension** is an attribute of a metric (such as filter with instance id, environment, etc...). 
    - Up to 10 dimensions per metric
    - Could be used to filter CW data
- Metrics have **timestamps** ; each data point have timestamps
- Can create CloudWatch dashboards of metrics
    - to display metrics in one places
    - could create a global dashboard where aggregated all regions' dashboards

### CloudWatch EC2 Detailed monitoring
- EC2 instance metrics have metrics “every 5 minutes”; for additional charge, could send metrics with 1 minute interval. 
- All EC2 instances send key health and performance metrics to CW. Could retrieve data even EC2 terminated. 
- With detailed monitoring (for a cost), you get data “every 1 minute”
- Use detailed monitoring if you want to more prompt scale your ASG!
- The AWS Free Tier allows us to have 10 detailed monitoring metrics
- **Note:** EC2 Memory usage is by default not pushed (must be pushed
from inside the instance as a custom metric)

#### Host-level metrics
- Default EC2 host-level metrics: CPU, network, disk, and status check
- Use the CW agent for OS-level metrics ike memory usage, processes, and CPU idle time.


### AWS CloudWatch Custom Metrics - use CW Agent
- Possibility to define and send your own custom metrics to CloudWatch
- Ability to use dimensions (attributes) to segment metrics
    - Instance.id
    - Environment.name
- Metric resolution:
    - Standard: 1 minute
    - High Resolution: up to 1 second (StorageResolution API parameter - Higher cost)
- Use API call **PutMetricData**
- Use exponential back off in case of throttle errors

### Alarms are used to trigger notifications for any metric
- Alarms can go to Auto Scaling, EC2 Actions, SNS notifications
- Various options (sampling, %, max, min, etc...)
- Set thresholds
- Alarm States:
    - OK
    - INSUFFICIENT_DATA
    - ALARM
- Period:
    - Length of time in seconds to evaluate the metric
    - High resolution custom metrics: can only choose 10 sec or 30 sec

### AWS CloudWatch Logs
- Applications can send logs to CloudWatch using the SDK
- CloudWatch can collect log from:
    - Elastic Beanstalk: collection of logs from application
    - ECS: collection from containers
    - AWS Lambda: collection from function logs
    - VPC Flow Logs:VPC specific logs
    - API Gateway
    - CloudTrail based on filter
    - CloudWatch log agents: for example on EC2 machines
    - Route53: Log DNS queries
- CloudWatch Logs can go to:
    - Batch exporter to S3 for archival
    - Stream to ElasticSearch cluster for further analytics
- CloudWatch Logs can use filter expressions
- Logs storage architecture:
    - Log groups: arbitrary name, usually representing an application. Log expiration policy should be defineda at this level.
    - Log stream: instances within application / log files / containers
- Can define log expiration policies (never expire, 30 days, etc..)
- Using the AWS CLI we can tail CloudWatch logs
- To send logs to CloudWatch, make sure IAM permissions are correct!
- Security: encryption of logs using KMS at the Group Level


### AWS CloudWatch Events
- Schedule: Cron jobs
- Event Pattern: Event rules to react to a service doing something
    - Ex: CodePipeline state changes
- Triggers to Lambda functions, SQS/SNS/Kinesis Messages
- CloudWatch Event creates a small JSON document to give information
about the change
