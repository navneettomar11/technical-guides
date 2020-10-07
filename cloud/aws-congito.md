# What is AWS Congito ?
Amazon Congito provides authentication, authorization and user management for your web and mobile apps. You user can sign in directly with a username and password, or through a third party such as Facebook, Amazon, Google or Apple.
The two main components of Amazon Congito are user pools and identity pools. User pools are user directories that provide sign-up and sign-in options for your app users. Identity pools enable you to grant your user access to other AWS services. You can identity pools and user pools separately or together.
**An Amazon Congito user pool and identity pool used together**
See the diagram for a common Amazon Congito scenario. Here the goal is to authenticate your user, and then grant your user access to another AWS service.
1. In the first step your app user signs in through a user pool and receives user pool token after a successful authentication.
2. Next, your app exchange the user pool tokens for AWS credentials through an identity pool.
3. Finallym your app user can then use those AWS credentials to access other AWS services such as Amazon S3 or DynamoDB.

![AWS Congito Scenario](images/scenario-cup-cib2.png)

AWS Conginto is compliant with `SOC 1-3`, `PCI DSS`, `ISO 27001` and is `HIPPA-BAA` eligible.

## Features of Amazon Conginto

### User pools
A user 

