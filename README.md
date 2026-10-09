# AWS VPC Infrastructure with CloudFormation

A hands-on networking project using AWS CloudFormation to define public
and private subnets, routing and outbound internet access.

## Resources

- One VPC
- Two public subnets in different Availability Zones
- One private subnet
- Internet gateway
- Public and private route tables
- NAT gateway with an Elastic IP

## How it works

The public subnets use a default route to the internet gateway.
The private subnet uses a default route to the NAT gateway, allowing
outbound internet connections without assigning instances public IPs.

## Deployment

Requires an AWS account, configured AWS CLI and permissions to create
the resources.

Validate the template:

```bash
aws cloudformation validate-template \
  --template-body file://vpc.yaml \
  --region eu-north-1
```

Deploy:

```bash
aws cloudformation deploy \
  --template-file vpc.yaml \
  --stack-name portfolio-vpc \
  --region eu-north-1
```

## Verification

After deployment:
- Check that the stack reaches CREATE_COMPLETE.
- Inspect the VPC resource map and subnet associations.
- Confirm public routes point to the internet gateway.
- Confirm the private default route points to the NAT gateway.

This template creates networking resources only; it does not create
EC2 instances for connectivity testing.

## Design limitations

This learning project uses one NAT gateway and one private subnet.
It does not provide resilient private-subnet internet access across
multiple Availability Zones.

## Cleanup

The NAT gateway and public IPv4 address incur charges while provisioned.
Delete the stack after testing:

```bash
aws cloudformation delete-stack \
  --stack-name portfolio-vpc \
  --region eu-north-1
```# aws-vpc-cloudformation
AWS VPC infrastructure with public and private subnets, internet gateway and NAT gateway, built using CloudFormation.
