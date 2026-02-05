# AWS-Cloud-Financial-Pipeline
Infrastructure-as-Code (IaC) pipeline deploying an automated stock analysis system using AWS CloudFormation, EC2, Glue, and Google Gemini AI

# AWS Cloud Infrastructure: Financial Data Pipeline

**A fully automated ETL pipeline deployed via AWS CloudFormation (IaC) to collect, analyze, and catalog financial market data.**

## ☁️ Project Overview
This project demonstrates **Infrastructure as Code (IaC)** principles. Instead of manually configuring servers, I wrote a declarative `infrastructure.yaml` template that provisions the entire backend stack in minutes.

The system launches an **EC2 instance**, installs dependencies, configures a **Cron job** for daily execution, and sets up **AWS Glue Crawlers** to catalogue the data for downstream analytics. It integrates **Google Gemini AI** to perform sentiment analysis on financial news.

## 🏗️ Architecture
The CloudFormation template provisions the following resources:
1.  **Compute (EC2):** A `t2.micro` instance acting as the worker node.
2.  **Storage (S3):** A dedicated bucket (`stock-data-collection-bucket`) with `archive/` and `latest/` prefixes.
3.  **Automation (Cron):** A user-data script configures a daily Cron job (15:00 UTC) to run the collection script.
4.  **Data Catalog (AWS Glue):**
    * **Database:** `investment_dashboard_db`
    * **Crawlers:** Automatically infer schema changes for `financial_metrics` and `company_news`.
5.  **Security (IAM):** Least-privilege roles granting the EC2 instance access only to specific S3 paths and Glue resources.

## 🛠️ Tech Stack
* **Infrastructure as Code:** AWS CloudFormation (YAML).
* **AWS Services:** EC2, S3, IAM, Glue (Crawlers & Database).
* **Language:** Python 3.9 (Pandas, AWS Wrangler, Boto3).
* **AI Integration:** Google Gemini Pro (via `google-generativeai` SDK).
* **External APIs:** Yahoo Finance (`yfinance`).

## ⚙️ How It Works
1.  **Deployment:** The user uploads `infrastructure.yaml` to AWS CloudFormation.
2.  **Provisioning:** AWS spins up the EC2 instance and injects the Python script via `UserData`.
3.  **Execution (Daily):**
    * The script fetches live stock data for ~60 tickers.
    * It queries **Gemini AI** to generate sentiment scores (0-100) for company-specific news.
    * It saves the raw data to S3.
4.  **Cataloging:** AWS Glue Crawlers scan the S3 bucket to update the metadata tables, making the data instantly queryable via Amazon Athena.

## 📂 Repository Structure
* `infrastructure.yaml`: The master CloudFormation template.
* `src/stock_collector.py`: The extraction logic (Python) deployed to the EC2 instance.

## 🚀 Deployment Instructions
1.  Login to AWS Console -> **CloudFormation**.
2.  **Create Stack** -> "Upload a template file" -> Select `infrastructure.yaml`.
3.  Enter parameters:
    * `GeminiAPIKeyParameter`: Your Google AI Studio Key.
    * `YourIPAddressCIDRParameter`: Your IP for SSH access (e.g., `x.x.x.x/32`).
4.  Launch Stack.

## 👤 Author
**Brandyn Ewanek**
* [LinkedIn](https://www.linkedin.com/in/brandyn-ewanek-9733873b/)
* [Portfolio](https://github.com/Brandyn-Ewanek/)
