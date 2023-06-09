---
title: Cost Optimization
author: Prabs
date: 15-03-2023
---


### Cloudformation template for stopping and starting lower environment EC2 instances after business hours and before the business day begins.



## Why this stack ? 

* As your estate grows, you may need some automation to manage your operations and possibly reduce the time it takes to implement when business demands.

* You are only charged for the hours that the services are operational. This solution can help you reduce operational costs by stopping resources that are not in use and starting resources when capacity is needed. You can use the CloudFormation (CFN) template as explained below.

## What need to be done before you start
To configure and implement this solution, we use the following high-level features:

    * The key "schedule" and a value in the format <code>06:00-21:00</code> should be included in the EC2 instance tag
    * If you schedule to start your instance at 9AM, the time should be 09:00, not 9 or 9:00.
    * Keep in mind that lambda uses UTC timezone. Convert your hours to UTC based on your work location.
![tag](images/EC2_Tag_example.jpg)


## Configuration involved

    * Cloudformation
    * Python
    * Lambda
    * IAM
    * Eventbridge

## Steps

    The steps you need to follow to deploy the stack.

    1.  Select Create stack from the AWS CloudFormation console.
    2.  Select New Resources (standard).
    3.  Select Template is ready and then select Upload a template file.
    4.  Select Next after uploading the provided.yaml file.
    5.  Type <b>Name</b> into the Stack name field.
    6.  Select Next and, if necessary, provide tags.
    7.  Select Next and go over the stack details.
    8.  Because this template creates an IAM role and policy, check the acknowledgement box.
    9.  Select Create stack.
    10. To track the resource creation status, open the stack and navigate to the Resources tab.

<b> To delete all resources created by this template, navigate to the AWS CloudFormation console, select the stack, and then delete stack.</b>


## Stack DeepDive - EC2_start_stop.yaml

## Resources

## Stack DeepDive - EC2_start_stop.yaml


*	Configure the following tags in Amazon EC2: For example, key = schedule, value = HH:MM-HH:MM For example: 08:00-23:00 (use a '-' between the start and stop times). If you set the "schedule" tag to 09:00-23:00, the EC2 instance will starts at <b>9AM</b> and stops at </b>11PM</b>.
*   </b> AutoStopStartEC2Schedule </b> Cron is set to run hourly, for example, at 12:05, with subsequent runs at 13:05 and 14:05. You can change it based on your business requirements.
*   <b>LambdaEC2Role</b> – IAM Role to assume for lambda service
*   <b>LambdaStartStopEC2</b> – lambda serverless function with code written in Python3.9 as input
*   <b>StartStopEC2Rule</b> – Eventbridge rule to run this script at the specified time.
*   <b>StartStopEC2Rule</b>Modify the timings based on your business needs on eventbridge settings <code>cron(05 * * * ? *)</code> This scripts runs every hour after 5 mins .eg : <code> 01:05 AM, 02:05 AM, 03:05 AM </code>
*   <b>PermissionForEventsToInvokeLambda</b> – Permission to invoke lambda function by your eventbridgerule

