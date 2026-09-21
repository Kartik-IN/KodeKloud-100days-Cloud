# AWS Centralized Log Aggregation with VPC Peering

## Overview

This project implements centralized log aggregation across two AWS VPCs.
The private EC2 sends `/var/log/boots.log` to a public EC2 through VPC
Peering. The public EC2 uploads the log to Amazon S3 every five minutes.

## Architecture

``` text
Private VPC (10.10.0.0/16)
  Private EC2: 10.10.1.55
        |
        | SCP every 5 minutes
        v
VPC Peering: pcx-0cdc94bf8bda4aac9
        |
        v
Public VPC (10.20.0.0/16)
  Public EC2: 10.20.1.105
        |
        | AWS CLI every 5 minutes
        v
S3: nautilus-s3-logs-163624788
  nautilus-priv-vpc/boot/boots.log
```

## AWS Resources

### Private VPC

-   VPC: `vpc-0a53d6cd350056240`
-   CIDR: `10.10.0.0/16`
-   Subnet: `subnet-0d4495ce7ff6d9c40` (`10.10.1.0/24`)
-   Route table: `rtb-0bb9ba40cd04c352a`
-   EC2: `i-035747b29ac9726dc`
-   Private IP: `10.10.1.55`
-   Security Group: `sg-07552f475345042f9`

### Public VPC

-   VPC: `vpc-04fb737f578c9ea85`
-   CIDR: `10.20.0.0/16`
-   Subnet: `subnet-064a166a5a6b40567` (`10.20.1.0/24`)
-   Route table: `rtb-0c61904c14a8fd7b5`
-   Internet Gateway: `igw-0b8537907119b57e3`
-   EC2: `i-00ad3028ca2e811ae`
-   Private IP: `10.20.1.105`
-   Public IP: `3.223.127.156`
-   Security Group: `sg-0d12252e832f908dd`

### VPC Peering

-   Connection: `pcx-0cdc94bf8bda4aac9`
-   Status: `active`
-   Private route: `10.20.0.0/16 -> pcx-0cdc94bf8bda4aac9`
-   Public route: `10.10.0.0/16 -> pcx-0cdc94bf8bda4aac9`

### S3 and IAM

-   Bucket: `nautilus-s3-logs-163624788`
-   Object: `nautilus-priv-vpc/boot/boots.log`
-   IAM role: `nautilus-s3-role`
-   Instance profile: `nautilus-s3-profile`
-   Region: `us-east-1`

## S3 Setup

The S3 bucket was configured with public access blocked.

Required object:

``` text
s3://nautilus-s3-logs-163624788/nautilus-priv-vpc/boot/boots.log
```

## IAM Setup

The public EC2 uses the `nautilus-s3-role` instance role through
`nautilus-s3-profile`.

The role allows the instance to interact with S3. AWS CLI identity
verification confirmed the assumed role.

## VPC Peering and Routing

The two VPCs use non-overlapping CIDRs:

``` text
Private: 10.10.0.0/16
Public:  10.20.0.0/16
```

Routes were added in both directions:

``` text
10.10.0.0/16 -> 10.20.0.0/16 via VPC Peering
10.20.0.0/16 -> 10.10.0.0/16 via VPC Peering
```

Peering status was verified as `active`.

## SSH and Log Transfer

The SSH key `nautilus-key.pem` was placed on the required instances with
secure permissions.

Private-to-public SSH connectivity was verified successfully.

The private EC2 cron job:

``` cron
*/5 * * * * scp -i /home/ubuntu/nautilus-key.pem -o StrictHostKeyChecking=no /var/log/boots.log ubuntu@10.20.1.105:/home/ubuntu/log-transfer/boots.log >> /tmp/log-transfer.log 2>&1
```

The transferred file is stored on the public EC2 at:

``` text
/home/ubuntu/log-transfer/boots.log
```

## Public EC2 to S3

AWS CLI v2 was installed on the public EC2.

The public EC2 cron job:

``` cron
*/5 * * * * /usr/local/bin/aws s3 cp /home/ubuntu/log-transfer/boots.log s3://nautilus-s3-logs-163624788/nautilus-priv-vpc/boot/boots.log --region us-east-1 >> /tmp/s3-upload.log 2>&1
```

## Final Verification

The S3 object was verified using:

``` bash
aws s3api head-object   --bucket nautilus-s3-logs-163624788   --key nautilus-priv-vpc/boot/boots.log   --region us-east-1
```

Final verification showed:

``` text
ContentLength: 25 bytes
LastModified: 2026-09-20 06:40:03 UTC
ServerSideEncryption: AES256
```

The exact key was also confirmed with:

``` bash
aws s3api list-objects-v2   --bucket nautilus-s3-logs-163624788   --prefix "nautilus-priv-vpc/boot/"   --region us-east-1
```

Result:

``` text
nautilus-priv-vpc/boot/boots.log
```

The bucket location was verified as `us-east-1`.

## Security Verification

Temporary SSH access from the AWS client was used during setup and
removed afterward.

Final SSH ingress on the public EC2 was restricted to:

``` text
10.10.0.0/16
```

This allows the private VPC to reach the public EC2 without leaving the
temporary client SSH rule enabled.

## Final Checklist

-   [x] Private VPC
-   [x] Public VPC
-   [x] Public subnet and route table
-   [x] Internet Gateway
-   [x] Public EC2
-   [x] S3 bucket
-   [x] IAM role and instance profile
-   [x] VPC peering
-   [x] Bidirectional VPC routes
-   [x] SSH connectivity
-   [x] Private log transfer
-   [x] Public EC2 S3 upload
-   [x] Cron automation
-   [x] Exact S3 object verified
-   [x] Temporary client SSH access removed

## Outcome

The completed pipeline is:

``` text
/var/log/boots.log
      |
      | SCP every 5 minutes
      v
Public EC2
      |
      | AWS CLI every 5 minutes
      v
S3
  nautilus-priv-vpc/boot/boots.log
```

The centralized log aggregation workflow was successfully configured and
verified on the AWS side.
