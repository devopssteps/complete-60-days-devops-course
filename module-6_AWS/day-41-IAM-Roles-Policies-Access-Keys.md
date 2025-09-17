# day-41-IAM Roles, Policies, Access Keys

### Hands-on Demo: IAM Roles, Policies, Access Keys
1. IAM User & Access Keys
 - Go to IAM → Users → Add User
 - Choose Programmatic access (for CLI/SDK).
 - Attach a policy (e.g., AmazonS3FullAccess).
 - Download the Access Key ID & Secret Access Key.
 - Configure in CLI:
 ```sh
aws configure
```
Enter Access Key, Secret Key, region, and output format.
 - Test it:
```sh
aws s3 ls
```

### 2. IAM Policy (Custom)
 - Go to IAM → Policies → Create Policy.
 - Example JSON (allow only listing S3 buckets):
```sh
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    }
  ]
}
```
 - Attach this policy to a user.
 -  Test with CLI → user can list buckets but cannot create/delete.


### 3. IAM Role
 - Go to IAM → Roles → Create Role.
 - Select AWS service → EC2.
 - Attach a policy (e.g., AmazonS3ReadOnlyAccess).
 - Launch an EC2 instance → Attach this role.
 - From EC2, run:
```sh
aws s3 ls
```
(No access key needed — role provides temporary credentials.)
✅ This demo covers users + access keys, custom policies, and roles in AWS IAM.
