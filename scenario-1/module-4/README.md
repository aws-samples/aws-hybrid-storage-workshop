#  Cutover data volume to a remote location eu-west-2

## Introduction

In this module, you will cutover to a tertiary region (London eu-west-2), which could very well be anywhere, including a private DR datacenter. In this module you will deploy a new gateway, a clone on the first gateway volume from module 2 and windows OS (instance) that will mount the volume. This module demonstrates how you can clone gateway volume hosted in an AWS region and present it anywhere that has internet connectivity and the ability to host the gateway OS on a supported hypervisor (Amazon EC2, VMware, Hyper-V).

You can launch this AWS CloudFormation template in the eu-west-2 region to build the necessary resources automatically.

## Architecture overview

![scenario-1-cutover-1](../../images/scenario-1-cutover-2.png)

### 1.	Deploy Gateway & Windows Instance with EBS volume using CloudFormation in Frankfurt (eu-central-1)

<details>
<summary><strong>CloudFormation Launch Instructions (expand for details)</strong></summary><p>

1.	Right click the **Launch Stack** link below and "open in new tab"

Region| Launch
------|-----
EU (London) | [![Launch Module 1 in eu-west-2](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/new?stackName=storage-workshop-1d&templateURL=https://s3-us-west-2.amazonaws.com/hybrid-storage-workshop/scenario1-step4-cutover2-SGW2-WIN3-(eu-west-2).json)

2. Click **Next** on the Select Template page.
3. Select your default VPC and any one of the subnets within that VPC.
4. Leave the Windows Instance Type as t2.medium
5. Leave the Gateway Instance Type as c4.2xlarge
6. Leave the cache and upload buffer sizes as 10GiB
7. Leave the activation region as (eu-central-1), which is where our volume data resides.
8. Select the key pair from the last module
9. Leave the **Allow DRP access from** field as 0.0.0.0/0 or enter the public IP of the computer from which you plan to access the Windows server.  You can find your public IP address at http://www.whatismypublicip.com/

![scenario-1-module-4-Picture1](../../images/scenario-1-module-4-Picture1.png)

10. Click **Next**.
11. Click **Next**. (skipping IAM advanced section)

8.	On the Review page, check the box to acknowledge that CloudFormation will create IAM resources and click **Create**.

![scenario-2-module-1-Picture1](../../images/scenario-2-module-1-Picture1.png)

Once the CloudFormation stack shows a status of CREATE_COMPLETE, you are ready to move on to the next step.

## 2. Connect the EC2 instance in Frankfurt eu-central-1 via RDP

<details>
<summary><strong>Connect to your EC2 instance (expand for details)</strong></summary><p>

1.	From the AWS console, click **Services** and select **EC2**  
2.	Select **Instances** from the menu on the left.
3.	Wait until the newly create instance shows as *running*.

![scenario-1-module-3-Picture2](../../images/scenario-1-module-3-Picture2.png)

4. Right click on your newly provisoined instance and select **connect** from the menu.
5. Click **Get Password** and use your .pem to access the RDP administrator password. Keep a copy of the password for your RDP client.
6. Click **Download Remote Desktop File** and open the file with your RDP client
7. Use the password from step 5 to authenticate and connect your RDP client to your windows instance

Note: For detailed instructions on How To connect to your Windows instance using an RDP client ([Connecting to Your Windows Instance](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html))

![scenario-1-module-3-Picture3](../../images/scenario-1-module-3-Picture3.png)

After Windows console has launched, open Disk Management by right clicking the Windows logo in the lower-left corner and select the **Disk Management**. You will see a new Offline Disk 1. This contains a copy of the volume from the Volume Gateway you deployed in module 2. Bring the volume online by right-clicking the section describing the disk and selecting **Online**.
</p></details>

## Validation Step

<details>
<summary><strong>Verify sample data exists on your EC2 instance (expand for details)</strong></summary><p>

Check the new D: drive in File Explorer and you should see all the data that was on the original volume that was cloned.

![scenario-1-module-1-Picture5](../../images/scenario-1-module-1-Picture5.png)

### What just happened?

This is a method of migrating data, using an EBS snapshot of the Volume Gateway volume, enables minimal downtime during cutover to AWS since all of the data already resides at AWS. This is optimal for large data drives that exist on file servers, database servers, web servers and any other system that needs to store large amounts of data locally. 

In this module, a new Windows EC2 instance was launched in AWS (eu-central-1 region) with the migrated data mounted from an EBS snapshot that you created from the Volume Gateway volume which was being hosting in the Frankfurt region (even when it was being presented to Ireland region via the EC2 gateway in that region).

You now have a Windows instance in eu-central-1 that contains a boot volume and a data volume. The secondary volume is a copy of the data that was hosted by the gateway volume in module 2 (drive E:). At this point you have successfully migrated data from a region simulating an on-premises deployment to the Frankfurt eu-central-1 region. 

</p></details>

### Start next module

Module 4: [Cutover data volume to a remote location eu-west-2](../module-4/README.md)

## License

This library is licensed under the Amazon Software License.

[Back to the main workshop scenarios page](../../README.md)
