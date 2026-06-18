# 🚀 EC2 MariaDB to RDS MySQL Migration using AWS DMS

[![GitHub stars](https://img.shields.io/github/stars/mdmasumhowlader/aws-dms-ec2-mariadb-to-rds-mysql)](https://github.com/mdmasumhowlader/aws-dms-ec2-mariadb-to-rds-mysql/stargazers)
[![GitHub license](https://img.shields.io/github/license/mdmasumhowlader/aws-dms-ec2-mariadb-to-rds-mysql)](https://github.com/mdmasumhowlader/aws-dms-ec2-mariadb-to-rds-mysql/blob/main/LICENSE)

## 📘 Project Overview

This project demonstrates a complete, hands-on migration of a MariaDB database from an Amazon EC2 instance to a fully managed Amazon RDS for MySQL instance using AWS Database Migration Service (DMS). The migration was performed with near-zero downtime by using a combination of **Full Load** followed by **Change Data Capture (CDC)**.

## 📌 Key Features

- **Infrastructure Setup:** Launch and configure an EC2 instance, install MariaDB, and set up an RDS instance.
- **DMS Configuration:** Deploy a DMS replication instance, create source and target endpoints, and a migration task.
- **Zero-Downtime Strategy:** Implement CDC after the full load to capture ongoing changes.
- **Real-World Troubleshooting:** Documented a real challenge encountered: an "Access Denied" error on the `awsdms_control` database and its solution.
- **Portfolio Documentation:** This project includes a comprehensive implementation plan, a detailed step-by-step guide, and full database schema.

## 📊 Architecture

Source: EC2 Instance (MariaDB:3306)  ────▶    AWS DMS (Replication)     ───▶    Target: RDS (MySQL:3306)                                         


## 🧭 Migration Phases

1.  **Phase 1: Environment Setup:** Provisioned EC2 (t3.medium, Amazon Linux 2023) and RDS MySQL (8.4.8, db.t3.micro) instances.
2.  **Phase 2: Pre-Migration Configuration:** Configured binary logging on the source database and created DMS users with necessary privileges.
3.  **Phase 3: Full Load Migration:** Initiated the DMS task to copy existing data to the target RDS database.
4.  **Phase 4: CDC & Troubleshooting:** Started ongoing replication, identified and fixed a critical "Access Denied" error preventing CDC from starting.
5.  **Phase 5: Data Validation:** Verified row counts and critical data elements to ensure 100% data integrity.

## 🐛 Challenges & Solutions

**Challenge:** The CDC phase failed with the error: `Access denied for user 'dms_user'@'%' to database 'awsdms_control'`.

**Root Cause:** The DMS user on the target RDS instance lacked the necessary privileges to create and manage internal DMS control tables.

**Solution:** Granted the required `ALL PRIVILEGES` on the `awsdms_control` database and the target database, and then restarted the task.

```sql
GRANT ALL PRIVILEGES ON awsdms_control.* TO 'dms_user'@'%';
GRANT ALL PRIVILEGES ON dms_test.* TO 'dms_user'@'%';
FLUSH PRIVILEGES;
```

📸 Screenshots
Database ERD
https://assets/screenshots/ERD.png

DMS Migration Task Status
https://assets/screenshots/migration-task-status.png

Pre-Assessment Results After Fixes
https://assets/screenshots/pre-assessment-result-after-fixation.png


🛠️ Technologies Used
Cloud: Amazon Web Services (AWS)

AWS Services: DMS, EC2, RDS, VPC, CloudWatch

Databases: MariaDB 10.5, MySQL 8.4.8

Tools: bash, mysqlsh, AWS Management Console


🚀 Getting Started
Prerequisites
AWS Account with access to DMS, EC2, and RDS

Basic knowledge of AWS Management Console

MariaDB/MySQL familiarity



Quick Start
Clone this repository

bash
git clone https://github.com/mdmasumhowlader/aws-dms-ec2-mariadb-to-rds-mysql.git
Review the Implementation Plan

Follow the Technical Guide

Use the Database Schema for testing


📂 Project Structure
text
aws-dms-ec2-mariadb-to-rds-mysql/
├── README.md                      # Project documentation
├── LICENSE                        # MIT License
├── docs/                          # Project documentation
│   ├── DMS-Implementation-Plan.pdf
│   ├── DMS-Project-Overview.pdf
│   └── DMS-EC2-MySQL-to-RDS.pdf
├── sql/                           # Database scripts
│   └── database-schema.sql        # Complete schema and test data
└── assets/
    └── screenshots/               # Screenshots
        ├── ERD.png
        ├── migration-task-status.png
        └── pre-assessment-result-after-fixation.png


🔗 Project Links
Case Study: View on Portfolio

Implementation Plan: docs/DMS-Implementation-Plan.pdf

Project Overview: docs/DMS-Project-Overview.pdf

Technical Guide: docs/DMS-EC2-MySQL-to-RDS.pdf

Database Schema: sql/database-schema.sql


📝 License
This project is licensed under the MIT License - see the LICENSE file for details.


⭐️ If you found this project helpful or interesting, please consider giving it a star on GitHub!
Author: Md. Masum Howlader
GitHub: mdmasumhowlader
LinkedIn: md-masum-howlader