# 🚀 EC2 MariaDB to RDS MySQL Migration using AWS DMS

[![GitHub stars](https://img.shields.io/github/stars/mdmasumhowlader/aws-dms-ec2-mariadb-to-rds-mysql)](https://github.com/mdmasumhowlader/aws-dms-ec2-mariadb-to-rds-mysql/stargazers)
[![GitHub license](https://img.shields.io/github/license/mdmasumhowlader/aws-dms-ec2-mariadb-to-rds-mysql)](https://github.com/mdmasumhowlader/aws-dms-ec2-mariadb-to-rds-mysql/blob/main/LICENSE)

## 📘 Project Overview

This project demonstrates a complete, hands-on migration of a MariaDB database from an Amazon EC2 instance to a fully managed Amazon RDS for MySQL instance using AWS Database Migration Service (DMS) [citation:4]. The migration was performed with near-zero downtime by using a combination of **Full Load** followed by **Change Data Capture (CDC)** [citation:1][citation:7].

## 📌 Key Features

- **Infrastructure Setup:** Launch and configure an EC2 instance, install MariaDB, and set up an RDS instance [citation:1].
- **DMS Configuration:** Deploy a DMS replication instance, create source and target endpoints, and a migration task [citation:1][citation:7].
- **Zero-Downtime Strategy:** Implement CDC after the full load to capture ongoing changes [citation:1][citation:7].
- **Real-World Troubleshooting:** Documented a real challenge encountered: an "Access Denied" error on the `awsdms_control` database and its solution.
- **Portfolio Documentation:** This project includes a comprehensive implementation plan, a detailed step-by-step guide, and full database schema [citation:2][citation:4].

## 🧭 Migration Phases

1.  **Phase 1: Environment Setup:** Provisioned EC2 (t3.medium, Amazon Linux 2023) and RDS MySQL (8.4.8, db.t3.micro) instances.
2.  **Phase 2: Pre-Migration Configuration:** Configured binary logging on the source database and created DMS users with necessary privileges [citation:1].
3.  **Phase 3: Full Load Migration:** Initiated the DMS task to copy existing data to the target RDS database.
4.  **Phase 4: CDC & Troubleshooting:** Started ongoing replication, identified and fixed a critical "Access Denied" error preventing CDC from starting.
5.  **Phase 5: Data Validation:** Verified row counts and critical data elements to ensure 100% data integrity.

## 🐛 Challenges & Solutions

**Challenge:** The CDC phase failed with the error: `Access denied for user 'dms_user'@'%' to database 'awsdms_control'`.

**Root Cause:** The DMS user on the target RDS instance lacked the necessary privileges to create and manage internal DMS control tables [citation:1].

**Solution:** Granted the required `ALL PRIVILEGES` on the `awsdms_control` database and the target database, and then restarted the task.
```sql
GRANT ALL PRIVILEGES ON awsdms_control.* TO 'dms_user'@'%';
GRANT ALL PRIVILEGES ON dms_test.* TO 'dms_user'@'%';
FLUSH PRIVILEGES;