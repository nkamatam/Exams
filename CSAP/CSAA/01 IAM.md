## IAM

Lab : Creating the user and enabling the multi factor Authentication.
### Exam Tips:
* IAM is universal. It does not apply to regions at this time.
* The "root account" is simply the account that you use to create your AWS account. It has the complete admin access to your AWS environment.
* New users have no permissions when they are created
* New users are (optionally when given the programmatic access) given access key and id when they are created .
* Access Key Id and Access Key are not the passwords. They are intended for programmatic access only.
* You only can see the Access Key Id, Access Key and the Password only once at the time of creation.
* Always setup multi factor authentication for your root account.
* You can create and customise your password rotation policies
### Setting up the Billing Alarm - Lab
* This is done using the Cloudwatch.
* You create an SNS topic and subscribe to that topic from your email ( by accepting the link sent by AWS). 
* When you try to create an alarm you can choose "Billing" to be one of the options.




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4ODE1NjkyNDNdfQ==
-->