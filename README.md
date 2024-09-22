# Serverless-Architecture-assignment
## Automated Instance Management Using AWS Lambda and Boto3:

### 1.	Set Up EC2 Instances:

 #### 1. Create EC2 Instances:
   Create First Instance (Auto-Stop):
        Add Tags: Under Value, type ‘Auto-Stop’.
   Create second Instance (Auto-Start):
        Add Tags: Under Value, type ‘Auto-Start’.
#### 2. Set Up IAM Role for Lambda:
   Create IAM Role:  select AmazonEC2FullAccess policy.
   Give the role a name ‘LambdaEC2Role’ and click Create Role.

#### 3.	Create Lambda Function:
   Create the Function
   Function Name: EC2AutoStartStop.
   Runtime: Select Python 3.x.
   Permissions: Choose the IAM role LambdaEC2Role.
#### 4.	Write the Lambda Code:
   In the Lambda function code editor, paste the following Python code using Boto3:
''' import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')

    # Find instances with tag 'Action' = 'Auto-Stop'
    stop_instances = ec2.describe_instances(
        Filters=[
            {'Name': 'tag:Action', 'Values': ['Auto-Stop']}
        ]
    )

    # Find instances with tag 'Action' = 'Auto-Start'
    start_instances = ec2.describe_instances(
        Filters=[
            {'Name': 'tag:Action', 'Values': ['Auto-Start']}
        ]
    )

    stop_instance_ids = [instance['InstanceId'] 
                         for reservation in stop_instances['Reservations']
                         for instance in reservation['Instances']]
    
    start_instance_ids = [instance['InstanceId'] 
                          for reservation in start_instances['Reservations']
                          for instance in reservation['Instances']]

    # Stop 'Auto-Stop' instances
    if stop_instance_ids:
        ec2.stop_instances(InstanceIds=stop_instance_ids)
        print(f'Stopped instances: {stop_instance_ids}')
    
    # Start 'Auto-Start' instances
    if start_instance_ids:
        ec2.start_instances(InstanceIds=start_instance_ids)
        print(f'Started instances: {start_instance_ids}')
    
    return {
        'statusCode': 200,
        'body': 'Lambda executed successfully!'
    }
    '''
   Click Deploy to save the Lambda function
#### 5.	Test the Lambda Function
   Manually Invoke the Function
   Create a new test event
   Click Test again to run the function.
   Check EC2 Instances
   Go back to the EC2 Dashboard.
   Verify that the instance tagged with Auto-Stop is stopped, and the one tagged with Auto- 
   Start is started.
   


#### 2. Implement a Log Cleaner for S3:

#### 1.	Set Up the IAM Role
  Create a New Role:
  Attach the AmazonS3FullAccess policy for now.
  Attach the AWSLambdaBasicExecutionRole for logging in CloudWatch.
  Assign the Role to your Lambda function under the “Permissions” tab.

#### 2. Create a New Lambda Function
  Go to AWS Lambda Console
  Choose "Author from scratch":
  Name your function S3LogCleaner.
  Runtime: Choose Python 3.10.
  Set Permissions:
  Create a new IAM role with basic Lambda execution permissions.
  Attach the AmazonS3FullAccess policy for this exercise. In a real-world scenario, use a more    restrictive policy.

#### 3.	Write the Boto3 Code for Log Cleaner 




### 4. Schedule the Function with EventBridge:
  To ensure the Lambda function runs weekly, we will schedule it using AWS EventBridge
  Go to EventBridge:
  Open the Amazon EventBridge console.
  Click on Create rule.
  Configure Rule Details:
  Name: Name the rule WeeklyS3LogCleanup.
  Event Source: Choose Event Source as EventBridge Scheduler.
  Schedule:
  Choose the schedule expression as rate (7 days) to trigger the function weekly.
  Target:
  Add a Target.
  Select Lambda Function as the target and choose the Lambda function you created.





















