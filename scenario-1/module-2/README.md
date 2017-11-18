#  Migrate data to an AWS Storage Gateway volume

## Introduction

In this module, you will deploy a Volume Gateway that will allow your data to be replicated to the Frankfurt region (AWS). You will deploy the gateway into the Ireland region in the same Availability Zone as the Windows instance you launched in Module 1. The gateway is registered in the Frankfurt region, however, and that is where the data will be stored for all volumes created on this gateway. 

You will launch this AWS CloudFormation template in the eu-west-1 region to build the necessary resources automatically. This template will create a Storage Gateway in the Frankfurt (eu-central-1) region, but deploy it in EC2 in the Ireland region so it can be attached to your new Windows instance. The gateway will be activated by the template and the appropriate security group will be attached.

## Architecture overview

![scenario-1-diagram-2](../../images/scenario-1-diagram-2.png)

### 1.	Deploy Windows Instance using CloudFormation Template

<details>
<summary><strong>CloudFormation Launch Instructions (expand for details)</strong></summary><p>

1.	Right click the **Launch Stack** link below and "open in new tab"

Region| Launch
------|-----
EU (Ireland) | [![Launch Module 1 in eu-west-1](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=storage-workshop-1b&templateURL=https://s3-us-west-2.amazonaws.com/hybrid-storage-workshop/scenario1-step2-migrate-SGW1-(eu-west-1).json)

2. Click **Next** on the Select Template page.
3. Select your default VPC and any one of the subnets within that VPC.
4. If you already have an Access Key Pair for this region that you have access to, enter that key pair.  Otherwise, you will need to create a new key pair.  Instructions to create a new key pair.
5. Select a subnet from the drop-down list.
5. Leave Instance Type, Gateway Cache Disk Size and Gateway Upload Buffer Disk Size at default values.
6. Choose a size for your volume that will be created on the gateway. It should be large enough to hold the data that you have on the D: drive of win1 instance you created in Module 1.
7. Leave the Activation Region at eu-central-1
8. Select the keypair that you used in Module 1
9. Select the Security Group that was created in Module 1 named "storage-workshop-1a-win1SecurityGroup...". This will allow our windows instance network access (iSCSI) to the gateway that is soon to be deployed in the same VPC.
10.	Click **Next**.
11.	Click **Next** Again. (skipping IAM advanced section)
12.	On the Review page, check the box to acknowledge that CloudFormation will create IAM resources and click **Create**.

![scenario-1-module-1-Picture2](../../images/scenario-1-module-1-Picture2.png)

Once the CloudFormation stack shows a status of CREATE_COMPLETE, you are ready to move on to the next step.
</p></details>

## 2. Verify the gateway is active and get its IP Address

<details>
<summary><strong>Verify gateway and get IP (expand for details)</strong></summary><p>

1. From the **Services** drop-down, select **EC2**.
2. You should see a new c4.2xlarge instance with the name "Hybrid Workshop - Migrate - Gateway Server 1 (storage-workshop-1b)"  in a *running* state.
3. Refresh the instance view periodically (every 30 seconds) until you see the word *Activated* in the EC2 instance name.
4. From the Services drop-down, select **Storage Gateway**.

Note: You will not see the gateway that was just provisioned here. While, we deployed the gateway into EC2 in the EU (Ireland) region, the gateway was activated in the EU (Frankfurt) region, so that is where we will find the gateway, and that is where the data written to it will be stored.

5.	Click on **Ireland** in the upper-right corner and select **EU (Frankfurt)** from the list to switch the console to the eu-central-1 region.

You will now see the Gateway that you just provisioned listed. Verify that their is a gateway named "Hybrid-Workshop-Gateway-Server-1...." and its status is *Running*.

6.	Click on the gateway to reveal the Details tab below. From the Details tab, make note of the *IP address* of the gateway and write it below. (We will use that address to connect our windows client to the storage gateways iSCSI interface.

7.	Click Volumes from the left menu to see the volume that was created by the CloudFormation stack. The size should match what you specified in the configuration (1-5 GiB).
</p></details>

## 3. Connect the windows server to the gateway volume

<details>
<summary><strong>Connect windows to gateway (expand for details)</strong></summary><p>

1.	Now comes the fun part! We will now attach the volume from your Volume Gateway Service in Frankfurt to your Windows instance in Ireland, giving that instance access to both local cache storage in that Ireland at the same time as remotely writting all of its data to Frankfurt. 

2. Return to your Windows instance, and open the iSCSI Initiator utility by clicking the Windows logo in the bottom left corner and typing ‘iscsi’ and then clicking iSCSI Initiator from the search results.

3. Click ‘Yes’ if prompted to enable the iSCSI service in Windows

![scenario-1-module-2-Picture1](../../images/scenario-1-module-2-Picture1.png)

</p></details>



## Validation Step

<details>
<summary><strong>Verify (expand for details)</strong></summary><p>

</p></details>

### Start next module

Module 3: [Cutover data volume to Amazon EBS in eu-central-1](../module-3/README.md)

## License

This library is licensed under the Amazon Software License.

[Back to the main workshop scenarios page](../../README.md)
