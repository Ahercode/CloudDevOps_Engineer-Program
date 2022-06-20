# ✒️Project Introduction
As your final project, you'll be faced with a real scenario.

Creating this project will give you the hands-on experience you need to confidently talk about infrastructure as code. We have chosen a realistic scenario where you will deploy a dummy application (a sample JavaScript or HTML file) to the Apache Web Server running on an EC2 instance.

There will be two parts to this project:

__Diagram__: You'll first develop a diagram that you can present as part of your portfolio and as a visual aid to understand the __CloudFormation script__:.
Script (Template and Parameters): The second part is to interpret the instructions and create a matching CloudFormation script.

## 📂 Scenario
1. Your company is creating an Instagram clone called Udagram.

2. Developers want to deploy a new application to the AWS infrastructure.

3. You have been tasked with provisioning the required infrastructure and deploying a dummy application, along with the necessary supporting software.

4. This needs to be automated so that the infrastructure can be discarded as soon as the testing team finishes their tests and gathers their results.

## Server specs


1. You'll need to create a __Launch Configuration__ for your application servers in order to deploy four servers, two located in each of your private subnets. The launch configuration will be used by an auto-scaling group.
2. You'll need two vCPUs and at least 4GB of RAM. The Operating System to be used is Ubuntu 18. So, choose an Instance size and Machine Image (AMI) that best fits this spec.
Be sure to allocate at least 10GB of disk space so that you don't run into issues.

## Security Groups and Roles

- Since you will be downloading the application archive from an __S3 Bucket__, you'll need to create an __IAM Role__ that allows your instances to use the S3 Service.
- Udagram communicates on the default ``HTTP Port: 80``, so your servers will need this inbound port open since you will use it with the __Load Balancer__ and the __Load Balancer Health Check__. As for outbound, the servers will need unrestricted internet access to be able to download and update their software.
- The load balancer should allow all public traffic ``(0.0.0.0/0)`` on port ``80`` inbound, which is the default ``HTTP port``. Outbound, it will only be using ``port 80`` to reach the internal servers.
- The application needs to be deployed into private subnets with a __Load Balancer__ located in a public subnet.
- One of the output exports of the __CloudFormation__ script should be the public URL of the __LoadBalancer__. __Bonus points__ if you add ``http://`` in front of the load balancer __DNS Name__ in the output, for convenience.


## 📂 Other Considerations


1. You can deploy your servers with an SSH Key into Public subnets while you are creating the script. This helps with troubleshooting. Once done, move them to your private subnets and remove the SSH Key from your __Launch Configuration__.
2. It also helps to test directly, without the load balancer. Once you are confident that your server is behaving correctly, increase the instance count and add the load balancer to your script.
3. While your instances are in public subnets, you'll also need the ``SSH port open (port 22)`` for your access, in case you need to troubleshoot your instances.
4. Log information for UserData scripts is located in this file: ``cloud-init-output.log`` under the folder: ``/var/log``.
5. You should be able to destroy the entire infrastructure and build it back up without any manual steps required, other than running the CloudFormation script.
6. The provided UserData script should help you install all the required dependencies. Bear in mind that this process takes several minutes to complete. Also, the application takes a few seconds to load. This information is crucial for the settings of your load balancer health check.
7. It's up to you to decide which values should be parameters and which you will hard-code in your script.
8. See the provided supporting code for help and more clues.
9. If you want to go the extra mile, set up a bastion host (jump box) to allow you to SSH into your private subnet servers. This bastion host would be on a Public Subnet with ``port 22`` open only to your home ``IP address``, and it would need to have the private key that you use to access the other servers.
10. __Last thing__: Remember to __delete your CloudFormation stack__ when you're done to avoid recurring charges
