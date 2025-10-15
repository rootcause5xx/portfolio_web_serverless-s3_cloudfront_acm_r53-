# 🚀 Host Your Portfolio Website on AWS (S3 + CloudFront + ACM + Route53)

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazon-aws)
![S3](https://img.shields.io/badge/S3-Static%20Hosting-blue?logo=amazon-s3)
![CloudFront](https://img.shields.io/badge/CloudFront-CDN-green?logo=cloudflare)
![Route53](https://img.shields.io/badge/Route53-DNS-yellow?logo=amazon-route53)
![ACM](https://img.shields.io/badge/ACM-SSL%20Certificate-lightgrey)
![DevOps](https://img.shields.io/badge/DevOps-Automation-success?logo=githubactions)

A step-by-step guide to host your **static portfolio website** on AWS using **S3**, **CloudFront**, **ACM**, and **Route53** — completely secure, fast, and accessible through your own domain.

---

## 📑 Table of Contents

1. [✅ Prerequisites](#prerequisites)  
2. [🪣 Step 1: Create and Configure the S3 Bucket](#step-1-create-and-configure-the-s3-bucket)  
3. [🌐 Step 2: Secure and Accelerate Using CloudFront](#step-2-secure-and-accelerate-using-cloudfront)  
4. [🔐 Step 3: Request SSL Certificate from AWS ACM](#step-3-request-ssl-certificate-from-aws-acm)  
5. [🌍 Step 4: Connect CloudFront with Route53 (Custom Domain Setup)](#step-4-connect-cloudfront-with-route53-custom-domain-setup)  
6. [🛡️ Step 5: Verification and Optional Enhancements](#step-5-verification-and-optional-enhancements)  
7. [🎯 Final Outcome](#final-outcome)  
8. [🧠 Author](#author)  

---

## ✅ Prerequisites

- You have a **valid and active AWS account**.  
- You have your **portfolio web app code** ready (HTML/CSS/JS and assets).  
- You have pushed your code to **GitHub** (for version control and tracking).  
- You have a **domain name** (from Route53, Namecheap, GoDaddy, etc.).  

---

## 🪣 Step 1: Create and Configure the S3 Bucket

1. Go to the **AWS S3 console** and create a new bucket.  
   - Name it something meaningful (e.g., `portfolio.example.com`).  
   - Choose a region close to your target audience.  

2. **Uncheck “Block all public access”** so your website can be viewed publicly.  
   - Confirm the warning prompt.  

3. **Upload your website files** (HTML, CSS, JS, images) to the bucket.  

4. Go to each object’s **Permissions** → ensure it allows public read access, or add a **bucket policy** (recommended).  

   Example bucket policy:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "PublicReadGetObject",
         "Effect": "Allow",
         "Principal": "*",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::your-bucket-name/*"
       }
     ]
   }
````

5. Go to **Properties → Static website hosting**, enable it, and set:

   * **Index document:** `index.html`
   * **Error document:** `error.html` (optional)

6. Copy the **Bucket website endpoint** and test it in your browser.
   ✅ You should see your portfolio website live via S3!

---

## 🌐 Step 2: Secure and Accelerate Using CloudFront

1. Go to **AWS CloudFront** → Create a new distribution.

2. Under **Origin domain**, select your **S3 static website endpoint** (not the bucket ARN).

3. Configure:

   * **Viewer protocol policy:** Redirect HTTP to HTTPS
   * **Allowed HTTP methods:** GET, HEAD
   * **Cache policy:** Use managed `CachingOptimized`
   * **Enable AWS WAF** (optional but recommended)

4. Under **SSL certificate**, select:

   * **Custom SSL Certificate (ACM)** → choose your domain certificate (create one if not available).

5. Click **Create distribution** and wait for status → **Deployed**.

6. Once deployed, open the **CloudFront domain name** (e.g., `dxxxxx.cloudfront.net`).
   ✅ Your website should load securely via HTTPS.

---

## 🔐 Step 3: Request SSL Certificate from AWS ACM

1. Go to **AWS Certificate Manager (ACM)** → Request a **public certificate**.
2. Enter your domain name (e.g., `portfolio.example.com`).
3. Validate via **DNS (recommended)** if you use Route53.
4. After validation, return to **CloudFront** and attach this certificate to your distribution.

---

## 🌍 Step 4: Connect CloudFront with Route53 (Custom Domain Setup)

1. Open **Route53 → Hosted zones → yourdomain.com**.

2. Create a new **A record (Alias)**:

   * **Record name:** `portfolio` (or blank for root domain).
   * **Record type:** A (Alias).
   * **Alias target:** Select your **CloudFront distribution**.

3. Save the record.

✅ Now your domain `portfolio.example.com` points directly to your CloudFront distribution.
Try opening it in your browser — your site should load securely via HTTPS.

---

## 🛡️ Step 5: Verification and Optional Enhancements

* Confirm your site is **accessible via HTTPS** and redirects properly.
* Test access on both **desktop and mobile**.
* (Optional) Configure **WAF rules** for better security.
* (Optional) Set **CloudFront invalidations** for cache refresh when updating files.
* (Optional) Enable **S3 versioning** and **logging** for safety and visibility.

---

## 🎯 Final Outcome

Your **portfolio website** is now:

| Feature                          | Status |
| -------------------------------- | ------ |
| 🌍 Hosted on AWS                 | ✅      |
| 🔒 Secured with HTTPS            | ✅      |
| ⚡ Accelerated via CloudFront CDN | ✅      |
| 🧭 Custom domain via Route53     | ✅      |

You can now share your **GitHub repository** for others to follow this setup easily.

---

## 🧠 Author

**Aman Patel**
💼 *DevOps & Cloud Engineer | Passionate about automation and AWS*

---

⭐ *If you found this helpful, consider giving the repo a star!*

```
