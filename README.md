# project1-aws
Built a static website hosting project using AWS S3. I created a dedicated IAM user with limited S3 permissions and configured AWS CLI for secure access. Using CLI commands, I created the bucket, uploaded website files, and applied a bucket policy to allow public read-only access. Finally, I enabled static website hosting and validated the endpoint

Project: AWS S3 Static Website Hosting with Secure IAM Access
 IAM USER CREATION 
Why this step?
•	AWS root user should never be used for daily operations
•	Best practice is least-privilege IAM user
What I did
•	Logged in using root account
•	Created a dedicated IAM user for CLI usage
•	Granted only S3-related permissions
IAM Policy Used (Learning phase)
AmazonS3FullAccess
Access keys
•	Generated Access Key ID and Secret Access Key
•	Used them for AWS CLI authentication
________________________________________
 AWS CLI INSTALLATION & CONFIGURATION
Why AWS CLI?
•	Enables programmatic access
•	Core DevOps skill
•	Avoids console-only dependency
Installation (Windows)
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
Verify installation
aws --version
Configure CLI
aws configure
Entered:
AWS Access Key ID     : <access-key>
AWS Secret Access Key : <secret-key>
Default region name   : ap-south-1
Default output format : json
Verification (MOST IMPORTANT)
aws sts get-caller-identity
This confirms:
•	Credentials are valid
•	CLI is correctly authenticated
 
________________________________________
 S3 BUCKET CREATION 
Why S3?
•	Serverless
•	Highly available
•	Cost-effective
•	Ideal for static content
Bucket creation via CLI
aws s3 mb s3://project-statichosting
Bucket naming rules followed
•	Globally unique
•	Lowercase
•	No spaces
 
________________________________________
WEBSITE FILE UPLOAD USING AWS CLI
Local folder structure
project/
├── index.html
├── css/
├── images/
└── js/
Upload full website at once
aws s3 sync . s3://project-statichosting
Why sync?
•	Preserves directory structure
•	Efficient uploads
•	Industry best practice
________________________________________
STATIC WEBSITE HOSTING CONFIGURATION
Why?
•	Enables browser access
•	Converts S3 bucket into a web endpoint
Console steps
S3 → Bucket → Properties → Static website hosting
Configured:
Index document: index.html
Generated endpoint:
http://project-statichosting.s3-website.ap-south-1.amazonaws.com
________________________________________
PUBLIC ACCESS CONTROL (CRITICAL SECURITY STEP)
Why needed?
•	S3 buckets are private by default
•	Website won’t load without public read permission
Bucket Policy Applied
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadForStaticWebsite",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::project-statichosting/*"
    }
  ]
}
Applied using CLI
aws s3api put-bucket-policy \
  --bucket project-statichosting \
  --policy file://policy.json
Block Public Access settings
Disabled:
•	Block public bucket policies
•	Block public access through ACLs
________________________________________
 VALIDATION & TESTING
Website tested using:
S3 Website Endpoint
http://project-statichosting.s3-website.ap-south-2.amazonaws.com/
Confirmed:
•	HTML loads
•	CSS loads
•	Images render
•	No permission errors
 

