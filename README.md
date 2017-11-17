## AWS Hybrid Storage Workshop

## License

This library is licensed under the Amazon Software License.

## Introduction

This workshop shows you how to work with the AWS storage gateway service in both file and volume mode. The storage gateway service is ideal for hybrid deployments and migration solutions are looking the bridge traditional on-premises servers and storage protocols with cloud native services such as Amazon S3 and Amazon EBS. The workshop is broken into two independent scenarios.

## Scenario 1 – Volume Gateway

In this scenario, you will use AWS Storage Gateway to migrate a block based volume from one AWS Region (eu-west-1) to other regions (eu-west-2 and eu-central-1). This will simulate the process you would follow to migrate data volumes from a non-AWS datacenter to AWS as well, with the eu-west-1 (Ireland) region representing the non-AWS datacenter. You will migrate the data using two different methods.

You will first migrate the data by copying it to an AWS Storage Gateway volume mounted in eu-central-1.  You will then mount a copy of the volume and its data to an EC2 instance in eu-west-2 by leveraging storage gateways ability to generate EBS snapshots from block data stored in S3 and EC2s ability to mount volumes generated from EBS snapshots.

![Volume Gateway Cutover Scenario 1 Architecture](images/scenario-1-cutover-1.png)

You will then deploy a second AWS Storage Gateway in a remote region (eu-west-2), and mount a volume cloned from the origin Volume Gateway that is hosted in eu-central-1 to an EC2 instance in the destination region. The effect is the same in both methods, you will have a Windows server in the destination region with a data volume that matches the data volume from the original region.

![Get started on scenario 1](scenario-1/)
