# Shared Bucket for Landing Zone file storage

This Add-On is designed to deploy and S3 bucket in the master account, that is used as a storage location for items
to be retrieved by other Landing Zone deployments. 

Example 1: For federation deployments, a metadata file can be stored here for collection and upload as an Identity 
Provider<br>
Example 2: Zip files to be used in Lambda deployments where the code doesn't fit inline in the Cloudformation template.

The bucket is deployed with default encryption, with the choice of AES256 or aws:kms, if kms is chosen the 3rd parameter of KMS
key id must be defined with a key that can be used by the Landing Zone process. 

# Parameters

OrganizationId - The root organization id. The default is to use the ssm parameter variable: $[alfred_ssm_/org/primary/organization_id]<br>
SSEAlgorithm - The server-side encryption algorithm for the bucket default encryption. default: "AES256"<br>
KMSMasterKeyID - If kms encryption is chosen, the kms key id to encrypt bucket data.