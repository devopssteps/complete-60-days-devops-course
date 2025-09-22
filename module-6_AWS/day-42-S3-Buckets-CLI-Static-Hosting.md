# Day-42: S3 Buckets, CLI, Static Hosting

### Hands-on Demo: S3 Buckets, CLI, Static Hosting
### 1. Create an S3 Bucket
```sh
aws s3 mb s3://my-devops-demo-bucket --region us-east-1
```
 - Or via AWS Console → S3 → Create Bucket.
 - Ensure unique bucket name + select region.
### 2. Upload Files via CLI
```sh
echo "Hello from S3" > index.html
aws s3 cp index.html s3://my-devops-demo-bucket/
```
Verify:
```sh
aws s3 ls s3://my-devops-demo-bucket/
```

### 3. Disable Block all public access
 - Go to Bucket → Permissions → Edit Block public access (bucket settings) → Uncheck Block all public access  →  Click Save Changes

### 4. Enable Static Website Hosting
 - Go to Bucket → Properties → Static Website Hosting → Enable.
 - 
### 5. Make Files Public (for Hosting)
 - Add a Bucket Policy like:
```sh
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-devops-demo-bucket/*"
    }
  ]
}
```
 - Now visit the endpoint:
```sh
http://my-devops-demo-bucket.s3-website-us-east-1.amazonaws.com
```
### ✅ This demo covers:
 - Creating & managing S3 buckets
 - Using AWS CLI for uploads
 - Enabling static website hosting


