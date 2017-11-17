# Deploy AWS Storage Gateway in File mode and integrate with S3

## License

This library is licensed under the Amazon Software License.

## Introduction

In this module, you’ll deploy AWS storage gateway in file mode in the same network as the the Linux instance created in module 1.  You will use AWS storage gateway’s ability to present an NFS mount point to a private data center and store files in AWS S3 in other regions, which is highly available and durable.

## Architecture Overview

![scenario 2 diagram 3](../../images/scenario-2-diagram-3.png)

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
EU (Ireland) | [![Launch Module 1 in eu-west-1](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=storage-workshop-2b&templateURL=https://s3-us-west-2.amazonaws.com/hybrid-storage-workshop/scenario2-step2-migrate-FGW1-(eu-west-1).json)

2.  Click **Next** on the Select Template page.
3.	Select the VPC and subnet where the Linux instance was created in module 1
4.	Select the security group that start with "storage-workshop-2a-linux1SecurityGroup" (This will allow the Linux instance network access to the storage gateway instance)
5.	Click **Next**.

![scenario-2-module-2-Picture1](../../images/scenario-2-module-2-Picture1.png)

6.	Click **Next** Again. (skipping IAM advanced section)
7.	On the Review page, check the box to acknowledge that CloudFormation will create IAM resources and click **Create**. 

Once the CloudFormation stack shows a status of CREATE_COMPLETE, you are ready to move on to the next step2

Note: it may take some time for the gateway to activate. You can see the activation status of the gateway in the Name of the EC2 instance in eu-west-1

</p></details>

### 2. Configure storage Gateway in eu-central-1 region

From the AWS Management Console, select **Storage Gateway** from within services and select EU (Frankfurt) as the region.  You should see a storage gateway already created and activated in previous step.

<details>
<summary><strong>(expand for screenshot)</strong></summary><p>

![scenario-2-module-2-Picture2](../../images/scenario-2-module-2-Picture2.png)
</p></details>

### 3. Create a file share connected to your primary S3 bucket 

A file share can be created on the storage gateway to be used by NFS client. The file share also connects to the S3 bucket where the data is actually stored in the form of objects. Unix file permissions for each file and folder are stored as object metadata within objects in S3.

<details>
<summary><strong>Step-by-step instructions (expand for details)</strong></summary><p>

1.	Select the gateway named "Hybrid-Workshop-File-Gateway-Server-1..." then click **Create file share**.
2.	In the Create file share wizard, select the storage gateway that is created, input the name of the first S3 bucket we created in first module, and select Create a new IAM role. Then click **Next**.

![scenario-2-module-2-Picture3](../../images/scenario-2-module-2-Picture2.png)

3.	Choose Next to review configuration settings. (Note: Leaving the defaults in this lab scenario is fine however, in real deployments consider limiting access by IP.)

![scenario-2-module-2-Picture4](../../images/scenario-2-module-2-Picture2.png)

4.	Review your file share configuration settings, and then click **Create file share**. After your file share is created, you can select the share see your file share settings in the file share Details pane at the bottom of the console.
</p></details>

### 4. Mount the bucket over NFS on your Linux instance



