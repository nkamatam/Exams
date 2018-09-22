## KMS - 101

### Amazon Key Management Service
* AWS KMS is a managed service that makes it easy for you to create and control the encryption keys used to encrypt your data.
* AWS KMS is integrated into other services like 
	* EBS
	* S3
	* Amazon Redshift
	* Amazon Elastic Transcoder
	* Amazon Work Mail
	* Relation Database Service
* Makes it easy to encrypt your data with encryption keys that you manage
* Lab:
	* Goto IAM
	* Create a new Group "myKMSGroup"
	* Give the Group the admin access 
	* Add a user "myKeyManager"
	* Add the user to the group
		* Person managing the keys permission (But does not have permissions to do Encrption/Decrypt)
	* add user myEncryptor
		* Person who manages the encryption/decryption
	* Add the user to the group
	* Keys are _regional_
	* Create a Key - myEncrypotionKey
	* Give permissio to manage to myKeyManager
	* Give permission for encruption to myEncryptor
	* We have successfully created our 
	* 
* 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjk1NDUxMDk4LC0xMDI2OTI5MTksODgzMj
Q4NjAxLDE2NTEyOTY0ODldfQ==
-->