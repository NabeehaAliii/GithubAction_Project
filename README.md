# Automated Deployment Pipeline using GitHub Actions, AWS S3, and CloudFront

## Overview
This project demonstrates an automated deployment pipeline built using **GitHub Actions**, **Amazon S3**, and **CloudFront**. The pipeline automates the process of deploying static website content (HTML, CSS, JavaScript) to an S3 bucket, enabling continuous delivery of updates to the website with every new commit to the GitHub repository. AWS CloudFront ensures global content delivery with optimized load times.

### Architecture Diagram:

![Blank diagram](https://github.com/user-attachments/assets/aa2d5e31-2ef8-47b6-b8af-633d70e239f4)


---

## Pipeline Workflow:
1. **Code Push to GitHub Repository**:
   - Developers push code (HTML, CSS, JavaScript) to a GitHub repository.
   
2. **GitHub Actions**:
   - The push event triggers a GitHub Actions workflow, which automatically:
     - Checks out the latest code.
     - Configures AWS credentials using IAM roles.
     - Deploys the updated code to the S3 bucket.
     - Invalidates CloudFront cache to ensure changes are live globally.

3. **AWS S3**:
   - The static website files are hosted in an Amazon S3 bucket.
   - AWS S3 ensures that the website is accessible globally.

4. **AWS CloudFront**:
   - AWS CloudFront ensures fast and secure global distribution of the website content, improving load times and reliability.

5. **AWS SSL for HTTPS**:
   - AWS SSL certificates secure the website, providing HTTPS for users accessing the site.

---

## Key Components:
1. **GitHub**: 
   - Version control system used to store and manage the codebase.
   
2. **GitHub Actions**:
   - CI/CD service used to automate the deployment process, ensuring that changes to the repository trigger a new deployment to S3.
   
3. **AWS S3 (Simple Storage Service)**:
   - Hosting service for storing static website files and making them publicly accessible.
   
4. **AWS CloudFront**:
   - Content delivery network (CDN) that distributes website content globally, reducing latency and improving performance.

5. **AWS IAM (Identity and Access Management)**:
   - AWS service used to manage user permissions and securely configure AWS credentials in GitHub Actions.

6. **AWS SSL**:
   - Provides SSL/TLS encryption for secure data transfer between the end-users and the website.

---

## Prerequisites:
- **GitHub Repository** with HTML, CSS, and JS files.
- **AWS Account** with an S3 bucket configured for static website hosting.
- **IAM Role** with the necessary permissions for S3 and CloudFront.
- **GitHub Actions Workflow** configured with AWS credentials and CloudFront distribution details.

---

## GitHub Actions Workflow Example:
```yaml
name: Portfolio Deployment

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v1

    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ secrets.AWS_REGION }}

    - name: Deploy to S3 bucket
      run: |
        aws s3 sync ./ s3://your-s3-bucket-name --delete

    - name: Invalidate CloudFront Cache
      run: |
        aws cloudfront create-invalidation --distribution-id ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID}} --paths "/*"
```

---

## Future Enhancements:
- Add CloudWatch for monitoring and logging.
- Implement more advanced security features like AWS WAF (Web Application Firewall).
- Automate scaling using AWS Elastic Load Balancing.

---

Feel free to contribute or modify the pipeline to suit your needs. For any questions, reach out!

---
