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
<img width="960" height="447" alt="Screenshot 2025-07-13 121145" src="https://github.com/user-attachments/assets/c72a3e4e-d7ba-42c3-b03b-f4c4410aca8f" />


🌐 3. Enabled Static Website Hosting
Once the bucket was ready, I enabled static website hosting.

In the bucket’s Properties tab:

I set the index document as index.html

Error document as index.html (for SPA behavior)

Screenshot:
<img width="960" height="444" alt="Screenshot 2025-07-13 121508" src="https://github.com/user-attachments/assets/493b4444-4e31-457f-abcb-ae703c132402" />


🔓 4. Allowed Public Access
Then, I made sure the bucket could serve files publicly:

Under Permissions > Block public access, I unchecked all blocking options.

I confirmed the warning message to allow public access.

Screenshot:
<img width="956" height="449" alt="Screenshot 2025-07-13 122131" src="https://github.com/user-attachments/assets/aca74714-10cb-4017-8be3-eb96a6912e40" />


👥 5. Set Object Ownership to Public
I updated the Object Ownership to allow public read access.

Enabled ACLs

Selected Bucket owner preferred

Screenshot:
<img width="959" height="443" alt="Screenshot 2025-07-13 122302" src="https://github.com/user-attachments/assets/d74d3607-b430-4d88-b822-c77b7872a89e" />


🛡 6. Attached Public Read Policy via AWS Policy Generator
To allow public access to files, I generated and attached a bucket policy using the AWS Policy Generator:

Effect: Allow

Principal: *

Action: s3:GetObject

ARN: arn:aws:s3:::my-unique-bucket-name/*

Screenshot:
<img width="960" height="446" alt="Screenshot 2025-07-13 130004" src="https://github.com/user-attachments/assets/becffcc8-1872-495f-aad2-674a5d8c4358" />
<img width="960" height="447" alt="Screenshot 2025-07-13 130122" src="https://github.com/user-attachments/assets/020a96e4-8c03-4946-a8fc-6bc959821ed0" />

🔑 7. Created Access Keys
For GitHub Actions deployment, I created Access Keys for the IAM user I set up earlier.

These keys were added securely in GitHub repository secrets.

Screenshot:

<img width="960" height="443" alt="Screenshot 2025-07-13 130939" src="https://github.com/user-attachments/assets/9dcc3f1b-9b9c-41c8-85c0-abcaa7f0eca7" />

⚙️ 8. GitHub Actions – Workflow Setup
I created a new workflow file in .github/workflows/main.yml in my repository.
This workflow is triggered every time I push changes to the main branch.

Here’s the workflow code I used:

name: production testing pipeline

on:
  push:
    branches: [main]

jobs:
  deploy:
    name: Build, Test & Deploy Portfolio
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Validate Files Exist
      run: |
        test -f Dummy-portfolio-code/index.html || (echo "index.html missing" && exit 1)
        test -f Dummy-portfolio-code/style.css || (echo "style.css missing" && exit 1)
        test -f Dummy-portfolio-code/script.js || (echo "script.js missing" && exit 1)

    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v2
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: 'ap-south-1'

    - name: Deploy to S3
      run: |
        aws s3 sync Dummy-portfolio-code/ s3://${{ secrets.AWS_S3_BUCKET_NAME }} --delete


🔐 9. Added GitHub Secrets
In GitHub, I navigated to:

Settings > Secrets and variables > Actions

Added these secrets:

AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY

AWS_S3_BUCKET_NAME

<img width="960" height="443" alt="Screenshot 2025-07-13 133752" src="https://github.com/user-attachments/assets/d8752cc5-d303-44a4-b2be-d343aec870fc" />

✅ Final Result
After pushing to main, the pipeline triggered successfully, validated the files, and deployed my static site to the S3 bucket.

Website Live Preview Screenshot:

<img width="960" height="477" alt="Screenshot 2025-07-13 152343" src="https://github.com/user-attachments/assets/9ae3ee01-be44-4e2a-bee6-1d8b4198ecdd" />
<img width="960" height="478" alt="Screenshot 2025-07-13 152412" src="https://github.com/user-attachments/assets/72a00c76-1c31-4877-b498-82d8667344f9" />
