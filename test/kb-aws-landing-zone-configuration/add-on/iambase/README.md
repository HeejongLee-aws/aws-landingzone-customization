Created by: kyeonjoo@

This add-on product create security audit role for human in case customer does not use SSO and use IAM Role.

It create security audit role in Security Account with Readonly policies.

Also, it create security audit readonly role in each account so that security audit can assume these roles from security account.

Deployment Steps]

1. copy iambase directory under add-on folder.
2. Change role prefix in case you want to change it.
3. Let CodePipeline pick these changes and deploy it.
