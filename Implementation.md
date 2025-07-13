🚀 AWS S3 Static Website Deployment with GitHub Actions CI/CD
This project demonstrates how I deployed a static website to AWS S3 with full CI/CD pipeline integration using GitHub Actions. Below are the steps I followed, along with screenshots of each configuration and execution step.

🛠 1. IAM User Creation
I started by creating a new IAM user in the AWS IAM Console with programmatic access.

I attached the AmazonS3FullAccess policy to allow full access to S3.

Screenshot:
<img width="960" height="446" alt="Screenshot 2025-07-13 120045" src="https://github.com/user-attachments/assets/b2165333-c2d6-4b07-b10b-c085c35c698c" />
<img width="960" height="427" alt="Screenshot 2025-07-13 120159" src="https://github.com/user-attachments/assets/ab9b32e6-2b54-4ef4-98cc-a1ee27708381" />


✅ 2. S3 Bucket Creation
Next, I went to the AWS S3 Console and created a new S3 bucket.

Bucket name: my-unique-bucket-name

Region: Asia Pacific (Mumbai) – ap-south-1

I kept the remaining settings as default.

Screenshot:


🌐 3. Enabled Static Website Hosting
Once the bucket was ready, I enabled static website hosting.

In the bucket’s Properties tab:

I set the index document as index.html

Error document as index.html (for SPA behavior)

Screenshot:

🔓 4. Allowed Public Access
Then, I made sure the bucket could serve files publicly:

Under Permissions > Block public access, I unchecked all blocking options.

I confirmed the warning message to allow public access.

Screenshot:

👥 5. Set Object Ownership to Public
I updated the Object Ownership to allow public read access.

Enabled ACLs

Selected Bucket owner preferred

Screenshot:

🛡 6. Attached Public Read Policy via AWS Policy Generator
To allow public access to files, I generated and attached a bucket policy using the AWS Policy Generator:

Effect: Allow

Principal: *

Action: s3:GetObject

ARN: arn:aws:s3:::my-unique-bucket-name/*

Screenshot:
