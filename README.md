# AWS VPC and EC2 Infrastructure with CloudFormation

This project defines an AWS network and three EC2 instances using
CloudFormation. It demonstrates public and private subnet routing,
bastion access and automated web server setup.

## Templates

### vpc.yaml
Creates:
- A VPC with two public subnets and one private subnet.
- An internet gateway for public-subnet internet connectivity.
- A NAT gateway for outbound connections from the private subnet.
- Public and private route tables with subnet associations.

### instances.yaml
Creates:
- A bastion host in a public subnet.
- A Linux web server in the private subnet.
- A Windows Server instance in the private subnet.
- Security groups controlling access to the instances.

## How it works

The bastion host provides an entry point for accessing private servers.
The Windows security group permits RDP traffic from the bastion
security group.

When the Linux web server starts, its UserData script installs nginx
and curl, downloads an index.html file from GitHub and starts nginx.
The private subnet needs working NAT connectivity for these downloads.

The web server has no public IP address. Access requires a connection
through the bastion, such as an SSH tunnel. This version does not
include an Application Load Balancer.

## Deployment order

1. Deploy vpc.yaml and wait for the stack to complete.
2. Update the mappings in instances.yaml with the resulting VPC and
   subnet IDs.
3. Check that the AMIs and EC2 key pair exist in the selected region.
   The Linux AMI must support the apt-get commands used by UserData.
4. Deploy instances.yaml.

## Current limitations

The instance template contains account-specific resource mappings.
These must be updated before deploying in another AWS account.

The security groups currently include unrestricted bastion SSH and
broad web-server access rules. Restrict these before deployment.

## Skills demonstrated

AWS networking, CloudFormation, EC2, Linux administration, security
groups and automated instance configuration.

## Cleanup

Delete the instances stack before deleting the VPC stack.
EC2 instances, NAT gateways, storage and public IPv4 addresses can
incur charges while provisioned.
