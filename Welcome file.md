## What does IAM give you?

* Centralized control of your AWS account
* Shared Access to your AWS account
* Granular Access
* Identity Federation ( including AD, Facebook, Linkedin etc)
* Multifactor Authentication
* Provides temporary access to Users/devices and services as necessary (Ex: S3 bucket or dynamodb database)
* Allows you to setup your own password rotation policy
* Integrates with many different AWS services
* Supports PCI/DSS compliance

## Critical Terms:
Users : End users (people)
Groups : A common set of users under on eset of permissions
Roles: You can create roles and assign them to AWS resources. 
Policies: Is a document that defines one (or more) permissions

## IAM Lab
AWS console consists of 
* Wizards
* Tutorials, labs, some of which are free
* Notice regions on right hand top side. Select the region closest to you
	* Regions may differ based on the services available at the location you are logging in to
	* If you cant find a service in a region try checking in an other region by selecting a different region on the list
* Services: Top left of the screen
	*	IAM listed under security, identity and compliance

IAM splash screen contains:
* A sign-in link to the console that can be customized
* List of  IAM resources (user, groups, roles, identity providers, customer managed policies)
* Security check list
	* Delete your root access keys (checked by default)
	* Activate MFA on your root account 
		* (to enable another level of auth. beyond your username pwd auth.
	* Create IAM users 
		* (best practice is to create individual user accounts and assign users to logical group based on their job function)
	* Use groups to assign permissions
	* Apply an IAM policy

#### Manage MFA device:
* To activate MFA device you first need to install AWS MFA-compatible application to user's smartphone, PC or other device. (Activation links are provided)
* Authenticate the device after downloading the app on your device (mobile/PC/other device). 
* Two codes authentication

#### Create IAM users:
* Select AWS access type contains two types/levels of access:
	* Programmatic access
		* (Enables an Access Key ID and a Secret Access Key for AWS API, CLI, SDK, and other development tools)
	* AWS management console access
		* (Enables a password that allows user to sign-in to the AWS management console)
		* Set permissions for the new user, 
			* add user to a group/copy permissions from existing user/attach existing policies directly/Create group
		* Click on Create group
		* Give a name and add policies to it from the list of default policies
				* Policy JSON contains data such as Version, Statement
				* Statement contain Effect, Action, Resource
		* Review and create user
		* On successful creation, a success message is shown and access key and Secret Access Key are displayed
		* Important to save the sceret access key. Need to regenerate if lost.
* To create new group, click on the Groups from the Menu on the LHS. Click on Create New Group button, give it a name, select policies to attach to group.  Review and create group.

* To add users to Group, click on the Users item on LHS menu, select user, click on Groups Tab, Add user to Groups button, select the group and click Add to Groups.
* To remove user, check the Groups Tab to remove
* To add/remove permissions to user, use the Permissions Tab

* Now go back to Dashboard on LHS menu
* 'Apply an IAM password policy' can be completed 
	* Click on Manage password policy'
	* set the rules for password policy
	* Click on Apply Password Policy 
* Go back to the Dashboard, you have 5 green ticks :-)

* What are IAM Roles?
	* IAM roles are a secure way to grant permissions to entities that you can trust. Example of entities include the following:
		* IAM user in another account
		* Application code running on an EC2 instance that needs to perform action on AWS resources
		* An AWS service that needs to act on resources in your account to provide its features
		* Users from a corporate directory who use identity federation with SAML
	* IAM roles issue keys that are valid for short durations, making them a more secure way to grant access.
	* Click on Create Role
	* Select the trusted entity
	* Select the service that will use this role
	* Search and select required policy from the list of default policies
	* Give the role name and description, and create
	* This role can now be applied to any EC2 instance that we create.

## IAM Summary
* IAM consists of users, groups, roles, policy documents
* Example policy document that gives full access to everything within AWS
	* {"Version": "2012-10-17",
	    "Statement": 
		[
		   {"Effect": "Allow",
		     "Action": "\*",
		     "Resource": "\*" }
		]
	  }
* We created a role that can be used by an EC2 instance to allow access to read and write files within S3
* IAM is universal. Does not apply to regions. Anything setup by IAM can be used by any region
* root account is simply the first account created when you setup the AWS account. It has complete Admin access
* New users have no permissions at all when they are first created.
* New users are assigned Access key and Secret access keys when they are first created. These are not keys to login to the AWS management console. They can be used to access AWS via the API of cmd line 
* Always setup MFA on your root account
* You can create and customize your own password rotation policies

## EC2 101
* Elastic Compute Cloud
#### What is EC2
	* Its a webservice which provides resizeable compute capacity on the cloud
	* So its just VMs in the cloud
	* Reduces the time required to obtain and boot server instances, allowing you to quickly scale capacity(CPU, RAM, etc..), both up and down.
	* EC2 completely changed the industry. It was led by Amazon
	* EC2 has changed the economics of cloud computing. Pay for the capacity that you actually use. Pay by the hour, minute..
	* EC2 provides developer the tools to build fault resiliant applications and isolate themselves from common failure scenarios.

#### EC2 (pricing) Options
	* On demand: 
		* Allows you to pay a fixed  rate by the hour(or by the second) with no commitment.
		* depends on the type of instance as well. Rate by the second presently only available for linux instances. May soon be introduced for Win as well
	* Reserved instances:
		* Make a resercvation with Amazon. Contract can be 1 or 3 years in length. 
		* May have to pay partly up front or entirely upfront. Price lower than hourly instances.
	* Spot based: 
		* Enables you to bid for whatever price you want for 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcyODU4Mzk2NywtMTY5NDQ3NDI2M119
-->