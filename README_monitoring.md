Monitoring & Alert management:
-

![aws_monitoring.png](files%2Faws_monitoring.png)

Monitoring is essential to keep track of the health and performance of systems, applications, and infrastructure. Monitoring allows us to detect issues early on and prevent potential problems from turning into major incidents. It provides insights into the usage, availability, and performance of resources, and helps us make informed decisions about capacity planning, resource allocation, and optimization.

The four golden rules of monitoring are:
-

- ### Monitor everything that matters: 

It's important to identify the critical components that impact your system's performance and availability and monitor them regularly. This includes infrastructure, applications, and services.

- ### Monitor proactively: 

Monitoring should be proactive rather than reactive. This means setting up alerts and notifications to detect issues before they become critical and impact the system's performance.

- ### Monitor continuously:

Monitoring should be continuous, with data collected and analyzed in real-time. This allows you to respond quickly to issues and prevent them from becoming more severe.

- ### Monitor intelligently: 

Effective monitoring involves analyzing the data collected and identifying patterns and trends to gain insights into the system's behavior. This helps in predicting and preventing future issues.

What is Alert Management?
-

Alert management is the process of managing alerts generated by the monitoring system. Alerts are notifications that are triggered when a predefined threshold or condition is met. Alert management involves setting up rules for when and how alerts are generated, configuring escalation policies, and taking appropriate actions in response to alerts.

What is SNS?
-

SNS (Simple Notification Service) is a service provided by AWS that enables you to send notifications and alerts to multiple subscribers or endpoints. It supports multiple protocols such as email, SMS, HTTP, HTTPS, and more.

What is SQS?
-

SQS (Simple Queue Service) is a message queue service that allows you to decouple and scale microservices, distributed systems, and serverless applications. It enables you to decouple the components of a cloud application, and allows components to operate independently and asynchronously.

Cloud watch to monitor AWS service/s.
-

CloudWatch is a monitoring and management service provided by AWS. It allows you to monitor resources and applications, collect and track metrics, collect and monitor log files, and set alarms. CloudWatch can be used to monitor AWS services such as EC2 instances, RDS databases, and Lambda functions. It provides real-time insights and can be integrated with other AWS services such as SNS and SQS to automate alerting and notification.

Setting up CloudWatch Alarm:
-

![cloudwatch_alarm.png](files%2Fcloudwatch_alarm.png)

1. Head to the `Simple Notification Service` dashboard.
2. Select `Subscriptions` and `create a subscription` (if you have not yet created one).
3. Select `Topics` and `Create a topic`.
4. Choose the Standard type, name the topic and then create it.
5. Once the topic is created, click on the `Create subscription` button to add a new subscription to the topic.
6. In the `Create subscription` dialog box, select `Email` as the protocol and enter your email address in the `Endpoint` field.
7. Click the `Create subscription` button to create the subscription.
8. You will receive an email with a confirmation link. Click on the link to confirm your subscription.
9. Head back to the EC2 dashboard and select your instance
10. Select `Actions/Monitor` and `troubleshoot/Manage CloudWatch alarms`.
11. Create alarm with `name` and Select your notification that you created.
12. Set the alarm threshold (in this case, >= 50% cpu utilisation), name the alarm and select `Create`.
13. Your Alarm is now set up on the instance

![alarm_created.png](files%2Falarm_created.png)

14. To check if alarm works you can:
- check your instance
- go to `Actions`.
- click `Monitoring & Troubleshoot` & click `Manage CloudWatch Alarms`
- now in alarm threshold you can change setting for example change `>= 50% cpu utilisation` to `<= 50% cpu utilisation`
- now you should receive an email and warning in `Cloud Watch alarms`:

![email_alarm.png](files%2Femail_alarm.png)

![alarm_not.png](files%2Falarm_not.png)




















