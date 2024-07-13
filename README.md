# IoT Infrastructure Optimization with AWS CloudWatch and Cost Management
This repository provides practical steps and Terraform scripts for optimizing IoT infrastructure using AWS CloudWatch for monitoring, alerting, and cost management strategies.

## Setup AWS CloudWatch Agent on Linux
### Install AWS CloudWatch Agent
  1. Download AWS CloudWatch Agent Package
     - Visit the AWS CloudWatch Agent download page and obtain the Linux package suitable for your distribution.
  2. Install the Agent
 
     Use yum for Amazon Linux, RHEL, or CentOS:
     ```
     sudo yum install amazon-cloudwatch-agent
     ```
     Use apt-get for Ubuntu or Debian:
     ```
     sudo apt-get install -y amazon-cloudwatch-agent
     ```
### Configure AWS CloudWatch Agent
  1. Create Configuration File
     - Create a configuration file (amazon-cloudwatch-agent.json) in /opt/aws/amazon-cloudwatch-agent/etc/:
       ```
       {
        "metrics": {
          "append_dimensions": {
            "InstanceId": "${aws:InstanceId}"
          },
          "metrics_collected": {
            "statsd": {},
            "mem": {},
            "swap": {},
            "disk": {},
            "diskio": {},
            "network": {},
            "processes": {},
            "awslogs": {}
          }
          }
        }
       ```
  2. Start AWS CloudWatch Agent
     - Start the AWS CloudWatch Agent service:
       ```
       sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json -s
       ```
### Create Custom Metrics (Optional)
  - Publish Custom Metrics
    Use the AWS CLI to publish custom metrics to CloudWatch. Replace <namespace>, <metric-name>, <value>, and <unit> with your specific metric details:
    ```
    aws cloudwatch put-metric-data --namespace "<namespace>" --metric-name "<metric-name>" --value <value> --unit "<unit>"
    ```
### Setting Up CloudWatch Alarms
  1. Navigate to CloudWatch Console
     - Go to the AWS Management Console and open the CloudWatch service.
     
  2. Create Alarm
     - Click on "Alarms" in the left sidebar and then "Create Alarm".

  3. Select Metric
     - Choose the metric you want to create an alarm for (e.g., CPUUtilization for EC2 instances).

  4. Define Alarm Conditions
     - Configure alarm conditions such as thresholds, periods, and actions:
      - Threshold: Set a threshold value (e.g., CPU utilization > 80%).
      - Period: Specify the evaluation period (e.g., 5 minutes).
      - Statistic: Choose statistical function (e.g., Average).
      - Actions: Define actions to be triggered when the alarm state changes (e.g., sending notifications).
  
  5. Add Notification Actions
     - Under "Actions", select "Create new topic" or choose an existing SNS topic.
     - Configure SNS topic settings and subscribe endpoints (e.g., email addresses) to receive notifications.

  6. Review and Create
     - Review your alarm configuration and click "Create Alarm" to activate the alarm.

### SNS Notification Setup
  1. Navigate to SNS Console
     - Go to the AWS Management Console and open the SNS service.
  2. Create Topic
     - Click on "Topics" in the left sidebar and then "Create topic".
     - Provide a name and display name for your topic (e.g., "CloudWatchAlarms").
  3. Subscribe Endpoints
     - Select your newly created topic and click "Create subscription".
     - Choose the protocol (e.g., email, SMS) and enter endpoint details (e.g., email address).
     - Confirm the subscription by responding to the confirmation message sent to the endpoint.
  4. Configure CloudWatch Alarms to Use SNS
     - Return to the CloudWatch console and edit your previously created alarm.
     - Under "Actions", select the SNS topic you created to receive alarm notifications.
  5. Save Changes
     - Save your changes to update the alarm configuration with SNS notification actions.

### AWS Cost Explorer
  1. Navigate to AWS Cost Explorer
     - Go to the AWS Management Console and open the Cost Management Console.

  2. Explore Costs
     - Use AWS Cost Explorer to analyze historical spending patterns, forecast future costs, and identify cost drivers.
     - Generate Cost Explorer reports based on different time ranges, services, and cost allocation tags.

### Cost Allocation Tags
  1. Implement Cost Allocation Tags
     - Tags help categorize and track costs associated with AWS resources.
     - Assign tags to resources (e.g., EC2 instances, S3 buckets) based on projects, departments, or environments.
     - Tags can be managed through the AWS Management Console or programmatically via AWS CLI/SDK.
