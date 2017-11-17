# Deploy Storage Gateway in File mode and integrate with S3

## License

This library is licensed under the Amazon Software License.

## Introduction

In this module, you’ll deploy AWS storage gateway in file mode in the same network as the the Linux instance created in module 1.  You will use AWS storage gateway’s ability to present an NFS mount point to a private data center and store files in AWS S3 in other regions, which is highly available and durable.

## Architecture Overview

![scenario 2 diagram 3](images/scenario-2-diagram-3.png)

The EC2 instance in eu-west-1 is to simulate the physical server in on-premises data center and a storage gateway is deployed on another EC2 instance to act as an on-premises file storage gateway. 

The Linux EC2 instance use NFS mount to connect to the file gateway.  Media files are copied to file server, which actually store all files in AWS S3 bucket in the other region. The cross region replication and lifecycle policy configured in module 1 will also be applied to the new S3 data.

## Implementation Instructions

### 1. Deploy Storage Gateway using CloudFormation Template

In order to give our Linux instance access to S3 over NFS, we first need to deploy a storage gateway in the same region as the Linux instance. To do this you can use the CloudFormation template below.

<details>
<summary><strong>CloudFormation Launch Instructions (expand for details)</strong></summary><p>

1.	Right click the **Launch Stack** link below and "open in new tab"

Region| Launch
------|-----
EU (Ireland) | [![Launch Module 1 in eu-west-1](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=storage-workshop-2a&templateURL=https://s3-us-west-2.amazonaws.com/hybrid-storage-workshop/scenario2-step2-migrate-FGW1-(eu-west-1).json)

2.  Click **Next** on the Select Template page.
3.	Select you’re the vpc and subnets where the Linux instance was created in module 1
4.	Select the security group that start with "storage-workshop-2a"
5.	Click Next.
