# AWS Hybrid Storage Workshop - Scenario 1 - Volume Gateway

## Prerequisites

### AWS Account

In order to complete this workshop, you will need an AWS account with access to create AWS IAM roles, EC2 instances, EBS volumes, AWS Storage Gateways and CloudFormation stacks in the eu-west-1, eu-west-2, and eu-central-1 regions.

Resources consumed as part of this workshop will have a cost and it is recommended that you follow the cleanup instructions once you have completed the workshop to remove all deployed resources and limit ongoing costs to your AWS account.

### Client Software

* **Browser** - We recommend that you use the latest version of Firefox or Chrome for this workshop.
* **RDP Client** - You will need an rdp client to access EC2 instances
* **AWS CLI** – You will need the aws cli installed on you client to access S3 objects
* **Key Pair** – You will need a valid eu-west-1 EC2 Key Pair. For more information on generating and downloading an EC2 Key Pair please visit [creating a key pair using amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair)

## Workshop Modules ###

This scenario is broken into 4 workshop modules:

* **Module 1** - [Deploy windows instance with sample data in eu-west-1](module-1/README.md)
* **Module 2** - [Migrate data to an AWS Storage Gateway volume](module-2/README.md)
* **Module 3** - [Cutover to volume data Amazon EBS](module-3/README.md)
* **Module 4** - [Cutover to volume data to a remote location](module-4/README.md)

## License

This library is licensed under the Amazon Software License.

[Back to the main workshop scenarios page](../../README.md)
