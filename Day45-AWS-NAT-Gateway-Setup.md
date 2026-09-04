# ☁️ Day 45 – AWS NAT Gateway Setup

## 📌 Task Overview

Configured a NAT Gateway to provide **outbound internet access** to the private EC2 instance `nautilus-priv-ec2` without assigning it a public IP.

## ☁️ Resources Created

| Resource | Name |
| --- | --- |
| VPC | `nautilus-priv-vpc` |
| Private Subnet | `nautilus-priv-subnet` |
| Public Subnet | `nautilus-pub-subnet` |
| Internet Gateway | `nautilus-igw` |
| Public Route Table | `nautilus-pub-rt` |
| Private Route Table | `nautilus-priv-rt` |
| Elastic IP | Allocated for NAT |
| NAT Gateway | `nautilus-natgw` |
| Private EC2 | `nautilus-priv-ec2` |
| S3 Bucket | `nautilus-nat-801155940` |

## ⚙️ Configuration

### 1. Public Subnet

Created the public subnet:

```text
nautilus-pub-subnet
```

Associated it with the public route table:

```text
nautilus-pub-rt
```

Configured the default route:

```text
0.0.0.0/0 → Internet Gateway
```

### 2. Internet Gateway

Created the Internet Gateway:

```text
nautilus-igw
```

Attached it to:

```text
nautilus-priv-vpc
```

### 3. NAT Gateway

Created the NAT Gateway:

```text
nautilus-natgw
```

The NAT Gateway was deployed in the public subnet and assigned an Elastic IP.

The NAT Gateway reached the following state:

```text
Available
```

### 4. Private Route Table

Created a dedicated private route table:

```text
nautilus-priv-rt
```

Explicitly associated it with:

```text
nautilus-priv-subnet
```

Added the default route:

```text
0.0.0.0/0 → nautilus-natgw
```

The VPC's main route table was not modified.

## 🔄 Final Network Flow

```text
nautilus-priv-ec2
        │
        ▼
nautilus-priv-subnet
        │
        ▼
nautilus-priv-rt
        │
        ▼
NAT Gateway
nautilus-natgw
        │
        ▼
nautilus-pub-subnet
        │
        ▼
nautilus-pub-rt
        │
        ▼
Internet Gateway
nautilus-igw
        │
        ▼
Internet / S3
```

## 🧪 Verification

Verified S3 connectivity with:

```bash
aws s3 ls s3://nautilus-nat-801155940/ --region us-east-1
```

Performed a manual upload test:

```bash
echo "NAT Gateway test" > /tmp/test.txt
aws s3 cp /tmp/test.txt s3://nautilus-nat-801155940/ --region us-east-1
```

Verified the uploaded file:

```bash
aws s3 ls s3://nautilus-nat-801155940/ --region us-east-1
```

The file appeared successfully in the S3 bucket.

## ✅ Result

- Public subnet configured
- Internet Gateway attached
- Public route table configured
- Elastic IP allocated
- NAT Gateway created and available
- Dedicated private route table created
- Private subnet associated with the private route table
- Default route configured through the NAT Gateway
- Private EC2 retains no public IP
- Internet and S3 connectivity verified
- S3 upload successfully tested
