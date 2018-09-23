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
	* We have successfully created our CMK (Customer MasterKey)
* The customer master key
	* alias
	* creation date
	* description
	* key state
	* Key Material (either customer provided or AWS created)
	* Can never be exported ( Can never leave AWS)
	* If you use cloud HSM, you can export your customer master key
* Setup CMK
	* Create Alias and description
	* Choose Material Option
	* Define Key administrative permissions
		* IAM users/roles can administer but not use (via KMS API)
* Define Key usage permissions
	* IAM users and roles that can use the key to encrypt and decrypt the data
* Key Material Options
	* AWS Generated
	* Your own key material
* echo "Hello" > secret.txt
* aws configure
	* Enter the keys
	* Region where the key is stored
* aws kms encrypt xxx
* aws kns decrype xxx
* aws kms re-encrypt xxx
* aws kms enable-key-rotation xxxx
**KMS Exam Tips**
* aws kms encrypt - Encrypts
* aws kns decrype - Decrypts
* aws kms re-encrypt - Re-Encrypts (it takes the encrypted text and reencrypt it - it would decrypt but destroy the decrypted one right away)
* aws kms enable-key-rotation -(what does it do? Rotaton every year?)

### Envelope Encryption
* Process of encrypting your envelope keys. They ecncrypt the key that encrypts the data.![Envelope Key](https://docs.aws.amazon.com/kms/latest/developerguide/images/key-hierarchy-cmk.png)

#### Decyption with AWS
![Decryption with AWS](https://github.com/nkamatam/Exams/blob/master/CDA/DecryptionWithAWS.png)

1. We have the envelop key
2. In order to decrypt that, we take our master key and decrypt that (using KMS API) - we use the encryption algorithm for this
3. So, we get the data key into plain text
4. Now, using this plain text data key, we decrypt the encrypted data
**Exam Tips**:
* Customer Master Key used to decrypt the data key(envelop key)
* Envelope key is used to Decrypt the data

#### KMS Exam Tips
* You can delete the key only in a 7 - 30 day window (disable first and then schedule the deletion)
* AWS KMS is a managed service that makes it easy for you to create and control the encryption keys used to encrypt your data.
* AWS KMS is integrated into other services like 
	* EBS
	* S3
	* Amazon Redshift
	* Amazon Elastic Transcoder
	* Amazon Work Mail
	* Relation Database Service

* The customer master key
	* alias
	* creation date
	* description
	* key state
	* Key Material (either customer provided or AWS created)
* Can never be exported ( Can never leave AWS) - Keys in HSM can be exported (HSM is dedicated to you??)

* Setup a Customer Master Key
* Key material options:
	* Use KMS generated key material
	* Your own key material
AWS KMS API
* aws kms encrypt - Encrypts
* aws kns decrype - Decrypts
* aws kms re-encrypt - Re-Encrypts (it takes the encrypted text and reencrypt it - it would decrypt but destroy the decrypted one right away)
* aws kms enable-key-rotation -(what does it do? Rotaton every year?)


* Customer Master Key used to decrypt the data key(envelop key)
* Envelope key is used to Decrypt the data










<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNDc3Njk5NTYsMjYzMjY0MTQ0LC00Mj
QyNjU1NjMsLTE4NDI3OTkzNDMsLTE3MzQ1NzcxNzIsMTU1OTE4
ODYzLC0xMDI2OTI5MTksODgzMjQ4NjAxLDE2NTEyOTY0ODldfQ
==
-->