# Cloud Security Monitoring Dashboard in AWS
Implementing a Cloud Security Monitoring Dashboard is a valuable project that enables real-time security monitoring, logging, and compliance checks for cloud environment.I will walk me through setting this up on AWS for consistency, but similar steps can be taken in GCP. By the end, i should be able to create a dashboard that monitors security events, integrates logging, sends alerts, and checks for compliance-allowing me to train my team with ease.


Project Overview: Cloud Security Monitoring Dashboard in AWS
I will achieve the following:
 1. Create a security Dashboard in AWS.
 2. Integrate CloudTrail for logging activities.
 3. Set up alerts for unauthorized access.
 4. Add compliance checkss for AWS resources.

Prerequisites
 1. AWS Account: I ensure that i have admin access to AWS or equivalent permissions
 2. AWS CLI & IAM Roles setup: AWS CLI configured on my local machine if needed, with necessary IAM permissions.
 3. Basic Cloud security knowledge: Familiarity with AWS CloudTrail, CloudWatch, IAM, and AWS security Hub.

Step 1: I create a Dashboard in AWS for security Monitoring
I will use AWS CloudWatch to create a dashboard to visualize our security metrics.

Step 1.1: I set up CloudWacth dashboard
1. Navigate to CloudWatch Dasboard:
   * Go to AWS Management console > cloudwatch > Dashboards.
   * Click Create dasboard, give it a name(e.g., securityMonitoringDashboard), and select Create.

2. Add Widgets for Security Metrics:
   * With the dashboard, click Add widget.
   * Select the Line graph or Number widget type, then add relevant metrics for security monitoring.
     Example: Widgets:
   * Unauthorized API Calls: In the metric search, go to Namespace >AWS/CloudTrail > EventName > UnauthorizedOperation.
   * Sign-in Failures: Use metrics from Namespace > AWS/IAM > SignInFailure.
   * Network Traffic: In Namespace > AWS/EC2 > NetworkIn and NetworkOut for EC2 instances to detect unusual activity.

  3. Configure and Save widgets: Customize each widget by setting filters, time ranges, and tresholds to highlight critical issues.

Step 2: Integrate CloudTrail for Activity Logging
Aws CloudTrail logs all account activities, which is essential for auditing and investigating potential security incidents.

Step 2.1: Set up CloudTrail logging
 1. Navigate to CloudTrail:
    * Go to AWS Management Console > CloudTrail.
    * Click Create trail and name it (e.g., OrganizationTrail if applying to all accounts).

 2. Configure Trail:
    * Applying trail to all accounts in organization (if i am managing multiple accounts)
    * Storage Location: Choose an S3 bucket (or create one specifically for CloudTrail logs).
    * Enable CloudWatch logs: Check Enable log delivery to cloudwacth logs, which alows us to create alerts later.

3. Select Events to Log:
   * Select Management events for high-level actions like creating/deleting resources.
   * Optionally, select Data events (e.g., for S3 or Lambda) for more granular tracking but keep in mind they can increase logging costs.
   * Enable Insights events for CloudTrail insights, which flags unusual activities automatically (usefull for detecting anomalies).

4. Review and Create: Confirm settings and create the trail.

Step 3: Set up Alerts for unauthorized Access Attempts
Using CloudWatch Alarms with SNS (Simple Notification Service), i will set up alert for suspicious activities, such as unauthorized API calls and failed sign-ins.

Step 3.1: Create SNS Topic for Alerts
   1. Navigate to SNS:
      * Go to AWS Management Console > SNS > Create topic.
      * Choose Standard type, name it (e.g., SecurityAlerts), and create it.
   2. Add subscriptions:
      * Within the topic, choose create subscription.
      * select Protocol as Email (or another alerting method) and provide an email address.
      * Confirm the subscriptio by clicking the link in the email.

  Step 3.2: Set up CloudWatch Alarms for Unauthorized Access
   1. Navigate to CloudWatch Alarms:
      * Go to AWS Mangement Console > CloudWatch > Alarms > Create alarm.

   2.Create Unauthorized API Call Alarm:
      * Metric: Choose the CloudTrail unauthorizedOperation metric from the AWS/CloudTrail namespace.
      * Set Conditions: Configure to trigger if the number of unauthorized calls exceeds a treshold (e.g., > 0).
      * Actions: Choose to Send a notification to the SNS topic (securityAlerts).
      * Review and Create.

  3. Create IAM Sign-in Failure Alarm:
      * Repeat the steps for CloudWatch Alarms, but select the AWS/IAM > SignInFailure metric.

  Step 4: Add Compliance Checks for Cloud Resources.
  Use AWS Security Hub and AWS Config for continuous compliance monitoring and best practice checks.

  Step 4.1: Enable Security Hub
     1. Navigate to Security Hub:
        * Go to AWS Management Console > Security Hub.
        * Click Enable Security Hub and select standards like CIS AWS Foundations or AWS Founctional Security Best Practices.

     2. Review Findings and Compliance Checks:
        * After enabling, security Hub will begin scanning my resources and flagging non-compliant ones.
        * Findings will be displayed in the Security Hub dasboard, categorized by severity and type.

  Step 4.2: Enable AWS Config Rules for Custom Compliance checks
    1. Navigate to AWS Config:
        * Go to AWS Management Console > Config > Rules.
        * Select add rule to create custom compliance checks.
    2. Select Rules for compliance:
        * Choose rules that align with my compliance requirements (e.g., s3-bucket-public-read-prohibited, ec2-instance-managed-by-systems-manager, cloud-trail-enabled).
        * Configure each rule as required and save.

    3. Set up Notification for compliance Failures:
        * Go back to CloudWatch Alarms to set alerts based on AWS Config rule non-compliance events.
        * I can set CloudWatch Alarms to monitor AWS Config changes and send alerts if a rule is violated.


    Summary of Implementation steps
     1. Dashboard Setup: Create a CloudWatch dashboard with widgets for real-time security metrics.
     2. CloudTrail Logging: Configure CloudTrail to log account activity and integrate with CloudWatch logs.
     3. Alerting: Set up SNS and CloudWatch Alarms for unauthorized access and sign-in failures.
     4. Compliance Monitoring: Enabled Security Hub and AWS Config for automated compliance checks and alerts.

     Example Execution of Each script and Command
Here are some CLI examples to automate setup:
    * Creating CloudWatch Dashboard with AWS CLI:
    aws cloudwatch put-dashboard --dashboard-name "SecurityMonitoringDashboard" \
    --dashboard-body '{"widgets":[{"type":"metric","x":0,"y":0,"width":6,"height":6,"properties":{"metrics":[["AWS/CloudTrail","UnauthorizedOperation"]],"title":"Unauthorized API Calls"}},{"type":"metric","x":6,"y":0,"width":6,"height":6,"properties":{"metrics":[["AWS/IAM","SignInFailure"]],"title":"Sign-In Failures"}}]}'


    * Enabling security Hub via AWS CLI:
    -> aws securityhub enable-security-hub --enable-default-standards

    * Creating CloudWatch Alarm for Unauthorized Access:
    -> aws cloudwatch put-metric-alarm --alarm-name "UnauthorizedAPICallAlert" \
    --metric-name "UnauthorizedOperation" --namespace "AWS/CloudTrail" \
    --statistic "Sum" --period 300 --threshold 1 --comparison-operator "GreaterThanThreshold" \
    --evaluation-periods 1 --alarm-actions "arn:aws:sns:REGION:ACCOUNT_ID:SecurityAlerts"

This Project integrates multiple AWS services to create a robust cloud security monitoring solution. Follow each section, test the configurations, and tweak based on my organization's requirements. Once implemented, i will be well-prepared to train my team on each componet and troubleshoot if any alerts or compliance issues arise.
     
