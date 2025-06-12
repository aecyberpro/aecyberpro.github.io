---
layout: post
title: AWS Role Assumption Credential Configuration
description: >
  I've documented an easy way to automatically configure AWS assumed role credentials. The AWS CLI handles all the complexity of assuming the role and credential management behind the scenes.
sitemap: true
---

# Introduction

There are two use cases where I typically need to assume a role, then configure the temporary credentials to use the assumed role:
1. My initial IAM user is locked down and requires MFA. I desired principle of least privilege instead of giving blanket admin rights to the IAM user. I have to use MFA to assume an admin role.
2. When performing AWS "whitebox" security assessments, my customer provides a cross-account role. I then need to assume the role to use it.

Once I assumed the role I needed to configure the assumed role temporary credentials in either environment variables or in a credentials file profile. I was looking for a way to streamline this and automatically configure the assumed credentials when I stumbled upon this information.

# Basic Role Assumption Configuration

In your `~/.aws/config` file, create a profile that references your base profile:

```ini
[profile base-profile]
region = us-east-1
output = json

[profile assumed-role-profile]
role_arn = arn:aws:iam::123456789012:role/YourTargetRole
source_profile = base-profile
region = us-east-1
```

In your `~/.aws/credentials` file, you only need the base profile credentials:

```ini
[base-profile]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
```

## How It Works

When you use the assumed role profile, the AWS CLI will:

1. Use the credentials from `base-profile`
2. Automatically call `sts:AssumeRole` to get temporary credentials
3. Use those temporary credentials for your AWS operations
4. Cache the credentials until they expire (default 1 hour)

## Usage Examples

```bash
# Use the assumed role profile
aws s3 ls --profile assumed-role-profile

# Or set it as your default profile
export AWS_PROFILE=assumed-role-profile
aws s3 ls
```

# Additional Configuration Options

You can also add:

```ini
[profile assumed-role-profile]
role_arn = arn:aws:iam::123456789012:role/YourTargetRole
source_profile = base-profile
region = us-east-1
role_session_name = MySessionName
duration_seconds = 3600
external_id = SomeExternalId  # If required by the role
mfa_serial = arn:aws:iam::123456789012:mfa/username  # If MFA is required
```

# For Cross-Account Roles

If you're assuming a role in a different account:

```ini
[profile cross-account-role]
role_arn = arn:aws:iam::999999999999:role/CrossAccountRole
source_profile = base-profile
external_id = UniqueExternalId  # Often required for cross-account access
```

# Alternative: Using credential_source

If you're running on EC2 instances or containers, you can use `credential_source` instead of `source_profile`:

```ini
[profile ec2-assumed-role]
role_arn = arn:aws:iam::123456789012:role/EC2AssumedRole
credential_source = Ec2InstanceMetadata
```

This approach eliminates the need to manually manage temporary credentials and makes role assumption seamless. The AWS CLI handles all the complexity of assuming the role and credential management behind the scenes.