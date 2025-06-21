Here is your updated `README.md` file with the **architecture diagram** added under the introduction section:

---

````markdown
# 🌐 Portfolio Website

This is a modern, responsive personal portfolio website built using HTML, CSS (or Tailwind), and optionally JavaScript or a frontend framework. It is automatically deployed to **Amazon S3** using **GitHub Actions** for Continuous Integration and Continuous Deployment (CI/CD).

---

## 📊 Architecture Diagram


![AWS Portfolio CI/CD Pipeline](architecture%20diagram.png)

This diagram illustrates the complete CI/CD workflow:
- User pushes changes to GitHub
- GitHub Actions triggers the deployment
- AWS IAM authenticates the GitHub runner
- Files are deployed to an S3 bucket for static website hosting

---

## 🚀 Features

- Clean, responsive design
- Hosted on **AWS S3**
- CI/CD pipeline using **GitHub Actions**
- Publicly accessible via S3 static website hosting
- Secure deployment using IAM user credentials

---

## 🔧 Technologies Used

- HTML / Tailwind CSS
- AWS S3 (Static Website Hosting)
- GitHub Actions (CI/CD)
- IAM (AWS Identity and Access Management)

---

## 🛠️ Deployment Setup

### 1. 🧱 AWS S3 Bucket Setup

- Create an S3 bucket (e.g., `my-portfolio-site`)
- Enable static website hosting
- Set **public read policy**:
  ```json
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "PublicReadGetObject",
        "Effect": "Allow",
        "Principal": "*",
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::my-portfolio-site/*"
      }
    ]
  }
````

---

### 2. 🔐 IAM User Setup

* Create an IAM user with **programmatic access**
* Attach the following policy (S3 Full Access for the specific bucket):

  ```json
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": "s3:*",
        "Resource": [
          "arn:aws:s3:::my-portfolio-site",
          "arn:aws:s3:::my-portfolio-site/*"
        ]
      }
    ]
  }
  ```
* Save the **Access Key ID** and **Secret Access Key**

---

### 3. 🔧 GitHub Repository Setup

* Add the following **secrets** in your GitHub repo settings:

  * `AWS_ACCESS_KEY_ID`
  * `AWS_SECRET_ACCESS_KEY`
  * `AWS_REGION` (e.g., `ap-south-1`)
  * `S3_BUCKET_NAME` (e.g., `my-portfolio-site`)

---

### 4. ⚙️ GitHub Actions Workflow

Create a file at `.github/workflows/deploy.yml`:

```yaml
name: Upload Portfolio

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Sync Files to S3
        uses: jakejarvis/s3-sync-action@master
        with:
          args: --acl public-read --follow-symlinks --delete
        env:
          AWS_S3_BUCKET: ${{ secrets.S3_BUCKET_NAME }}
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_REGION: ${{ secrets.AWS_REGION }}
```

---

## ✅ How It Works

1. Push code to `main` branch
2. GitHub Action runs and syncs files to your S3 bucket
3. Your website is immediately live!

---

## 🌍 Access Your Website

Visit:

```
http://<your-bucket-name>.s3-website-<region>.amazonaws.com/
```

Example:

```
http://my-portfolio-site.s3-website-ap-south-1.amazonaws.com/
```

---

## 🧹 Cleanup (Optional)

If you're done with the project:

* Delete the S3 bucket to stop storage charges
* Delete the IAM user or revoke its permissions
* Remove GitHub secrets and disable Actions

---

## 📄 License

This project is open-source and free to use for personal or educational purposes.

---

