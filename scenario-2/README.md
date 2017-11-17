# AWS Hybrid Storage Workshop - Scenario 2 - File Gateway

## License

This library is licensed under the Amazon Software License.

## Prerequisites

### AWS Account

In order to complete this workshop, you will need an AWS account with access to create AWS IAM, S3, EC2, Storage Gateway and Cloud Formation.

Resources consumed as part of this workshop will have a cost and it is recommended that you follow the cleanup instructions once you have completed the workshop to remove all deployed resources and limit ongoing costs to your AWS account.

### Client Software

* **Browser** - We recommend that you use the latest version of Firefox or Chrome for this workshop.
* **SSH Client** -You will need an ssh client to access EC2 instances
* **AWS CLI** – You will need the aws cli installed on you client to access S3 objects
* **Key Pair** – You will need a valid eu-west-1 EC2 Key Pair. For more information on generating and downloading an EC2 Key Pair please visit [creating a key pair using amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair)

## Workshop Modules ###

This scenario is broken into 2 modules:

* **Module 1** - [Deploy Linux Instance & configure S3 Storage Solution with CRR and Lifecycle Policy](module-1/README.md)
* **Module 2** - [Deploy Storage Gateway in File mode and integrate with S3](module-2/README.md)
