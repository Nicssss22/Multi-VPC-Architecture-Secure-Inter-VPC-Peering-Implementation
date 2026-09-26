<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Peering

**Project Link:** [View Project](https://nextwork.ai/projects/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16)

**Author:** agustinnico2302@gmail.com  
**Email:** agustinnico2302@gmail.com

---

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16_88727bef)

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC lets you create an isolated network in AWS to host and manage your cloud resources. You can shape the network by defining security controls, directing traffic, and grouping resources into subnets as your needs change.

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to initiate VPC peering connection, configure its route table to direct traffic between them, and troubleshoot network issues so that the connection between the resources of each VPCs will be successful.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was the need to accept the VPC invitation from the secondary VPC. I was facsinated in this feature because before peering connection to be established, there should be a permission which is a security control to validate if the request is expected. This is helpful to avoid non-permissive peering connections to any AWS accounts.

### This project took me...

This project took me for about 2-3hrs since I'd reverse engineered my VPC architecture to better understand the connection between the two (2) instances using the VPC peering connection.

## In the first part of my project...

### Step 1 - Set up my VPC

In this step, I will create two (2) VPCs from the scratch and use the VPC resource map the create this environment more efficiently.

### Step 2 - Create a Peering Connection

In this step, I will setup a connection link between my two (2) VPCs so my resources within these two VPCs can communicate with each other.

### Step 3 - Update Route Tables

In this step, I will configure my routing tables to setup a way for traffic coming from VPC 1 to get to VPC 2 and vice versa.

### Step 4 - Launch EC2 Instances

In this step, I will launch EC2 instance on each VPC to test the peering connection between them.

## Multi-VPC Architecture

I started my project by launching 2 VPCs using the VPC resource map and create one (1) public subnet for each.

The CIDR blocks for VPCs 1 and 2 are unique with each other. They have to be unique so that the resources within these VPCs wouldn't overlap.

### I also launched 2 EC2 instances

I didn't set up key pairs for these EC2 instances as we can initiate connection with EC2 Instance Connect. In this way, AWS will handle key pair management, while still maintaing secure connection.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16_11111111)

## VPC Peering

A VPC peering connection is a way to connect two (2) VPCs so their resources could communicate with each other. 

VPCs would use peering connections to establish connection between VPCs and transfer data and resources within the AWS network instead of routing the traffic via the public internet before arriving on other VPC. It is being used when businesses collaborate with each other to securely transfer resources between them instead of using the internet.

The requester is the VPC that initiates or requests a VPC peering connection. The accepter is the VPC that receives and accepts the request, making the peering connection active.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16_1cbb1b88)

## Updating route tables

After accepting a peering connection, I needed to update my VPCs’ route tables. By default, resources in one VPC don’t know how to reach resources in the other, so I added a route to each VPC’s public route table to direct traffic to the other VPC.

My VPC 1 new route have a destination of 10.2.0.0/16, which is the private CIDR block of the VPC 2. On the otherhand, the destination route of my VPC 2 is 10.1.0.0/24, which is the private CIDR block of the VPC 1. The routes' target for each VPC was the peering connection ID that was established before.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16_4a9e8014)

## In the second part of my project...

### Step 5 - Use EC2 Instance Connect

In this step, I will use the EC2 Instance Connect to connect to your first EC2 instance and troubleshoot any network problem to be encountered.

### Step 6 - Connect to EC2 Instance 1

In this step, I will use the EC2 Instance Connect to connect to Instance 1 after troubleshooting initial network issues and fix another error that I'm going to encounter again.

### Step 7 - Test VPC Peering

In this step, I will try to ping from the instance of the VPC 1 to the instance of VPC 2 so verify the connectivity between them.

## Troubleshooting Instance Connect

Next, I used EC2 Instance Connect to let AWS manage my key pair while maintaining secure connection to my EC2 instance directly through my AWS Management Console. 

I ran into an error with attempting to use the EC2 Instance Connect as there is no assigned public IPv4 address on my EC2 instance. It happened because EC2 Instance Connect uses the public internet when a client initiates a connection to an instance.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16_7685490c)

## Elastic IP addresses

To resolve this error, I set up Elastic IP addresses. An Elastic IP address is a static public IPv4 address that can be assigned to a resource, in my case, an EC2 instance. It stays allocated to my account until I disassociated or released it, and remains associated with the instance until I disassociate it. This gives applications a consistent address, so developers don’t need to update DNS records every time an instance restarts and receives a different public IPv4 address. It can also help avoid downtime while DNS changes propagate.

Associating an Elastic IP address resolved the error because a public IPv4 address is assigned to the instance which EC2 Instance Connect requires to make the connection successful.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16_45663498)

## Troubleshooting ping issues

To test VPC peering, I ran the command 'ping 10.2.4.236'. The IP address specified is the private address of the EC2 instance from the VPC 2.

A successful ping test would validate my VPC peering connection because it shows that the end-to-end connectivity is successful. Additionally, it also shows that the network configurations in the VPC such as the route table, Network ACL, and security group is correct.

I had to update my second EC2 instance's security group because the inbound rule for ICMP messages is not specified. So with this, I added a new rule that explicity allow that type of traffic.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16_7a29d352)

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16)*
