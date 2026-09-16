# Unit: Databases, Cloud Storage & Data Warehousing

This repository contains learning materials, core theoretical concepts, and hands-on practical guides covering relational databases, NoSQL systems, cloud storage architectures, data warehousing pipelines, and hands-on object storage operations using Cloudinary SDK.

---

## Table of Contents
1. [Introduction to Databases & SQL](#1-introduction-to-databases--sql)
2. [ACID Properties](#2-acid-properties)
3. [NoSQL Databases](#3-nosql-databases)
4. [Cloud Storage Solutions](#4-cloud-storage-solutions)
5. [Data Warehousing & ETL Concepts](#5-data-warehousing--etl-concepts)
6. [Practical Implementation with Cloudinary](#6-practical-implementation-with-cloudinary)
7. [Hands-on Lab Assignment](#7-hands-on-lab-assignment)

---

## 1. Introduction to Databases & SQL

Databases store, manage, and retrieve structured or unstructured data efficiently.

### Relational Databases (RDBMS)
Relational databases store data in predefined tables with rows and columns. They rely on **SQL (Structured Query Language)** for querying, schema definition, and data manipulation.

* **Characteristics:** Fixed schema, strong consistency, ACID compliance, support for complex relational joins.
* **Examples:** PostgreSQL, MySQL, Microsoft SQL Server, Oracle.

---

## 2. ACID Properties

To maintain reliability and data integrity during concurrent transactions, relational databases enforce **ACID** properties:

| Property | Description | Real-World Example |
| :--- | :--- | :--- |
| **Atomicity** | Either all operations in a transaction succeed, or the entire transaction is rolled back. | A ₹1000 bank transfer succeeds only if both debit and credit operations complete successfully. |
| **Consistency** | Data always moves from one valid state to another, strictly adhering to schema constraints. | Account balances cannot go negative if prohibited by schema check constraints. |
| **Isolation** | Concurrent transactions execute without interfering with or locking out one another. | Two users booking seats simultaneously process independently without corrupting state. |
| **Durability** | Once committed, data changes persist permanently even during unexpected system crashes. | Committed financial transactions survive total server power outages. |

---

## 3. NoSQL Databases

NoSQL databases are non-relational, distributed databases designed for horizontal scalability, high throughput, and flexible, schema-less data models.

### NoSQL Data Categories

* **Key-Value Stores:** Data is stored as individual key-value pairs. Optimized for ultra-fast lookup operations.
  * *Examples:* Redis, Amazon DynamoDB
* **Document Stores:** Data is encapsulated inside semi-structured documents (e.g., JSON or BSON format).
  * *Examples:* MongoDB, CouchDB
* **Columnar Databases (Column-Family):** Data is stored in columns instead of rows, heavily optimizing read operations for analytics.
  * *Examples:* Apache Cassandra, Google Cloud Bigtable
* **Graph Databases:** Uses nodes, edges, and properties to represent and query interconnected graph structures.
  * *Examples:* Neo4j, Amazon Neptune

---

## 4. Cloud Storage Solutions

Cloud storage stores files on remote cloud infrastructure accessible via web protocols or API calls instead of a local machine.

### Key Benefits
* Accessible from anywhere via internet API calls
* Scalable on-demand without manual infrastructure setup
* High availability and durability guarantees
* Seamlessly integrated with modern data pipelines

### Major Provider Overview

+-------------------+-----------------------+---------------------+
|      AWS S3       | Google Cloud Storage  | Azure Blob Storage  |
|  (Object Storage) |       (Buckets)       |     (Containers)    |
+-------------------+-----------------------+---------------------+


| Feature / Concept | AWS S3 | Google Cloud Storage | Azure Blob Storage |
| :--- | :--- | :--- | :--- |
| **Storage Unit** | Object | Object | Blob (File) |
| **Container Term** | Bucket | Bucket | Container |
| **Top Level Account** | AWS Account | GCP Project | Storage Account |
| **Best Used For** | General object storage, high availability across AZs | Analytics, Machine Learning workloads, global namespace | Enterprise data lakes, structured application storage |

### Essential Terminology
* **Bucket / Container:** Top-level logical storage container.
* **Object / Blob:** The individual file stored (CSV, JSON, Images, PDFs).
* **Key:** Full file path and object identifier (e.g., `student-data/employees.csv`).
* **Region:** Physical geographic datacenter location of storage servers.

---

## 5. Data Warehousing & ETL Concepts

A **Data Warehouse** is a centralized repository that stores historical, aggregated data gathered from multiple operational sources for decision-making and reporting.

### ETL Pipeline Architecture

+-----------------+      +---------+      +-----------+      +----------------+
|  Data Sources   | ---> | Extract | ---> | Transform | ---> | Data Warehouse |
| (SQL, CSV, API) |      |         |      | (Clean)   |      |   (Analytics)  |
+-----------------+      +---------+      +-----------+      +----------------+

### Core Characteristics
* Historical, read-optimized data structure
* Integrates disparate enterprise data sources
* Directly powers Business Intelligence (BI) dashboards
* *Popular Data Warehouses:* Snowflake, Amazon Redshift, Google BigQuery

### Pipeline Stages
1. **Extract:** Fetch raw records from transactional SQL databases, local CSVs, or web APIs.
2. **Transform:** Clean, filter, convert types, and restructure data into cohesive formats.
3. **Load:** Output transformed data into the central data warehouse.

---

## 6. Practical Implementation with Cloudinary

We use Cloudinary via Node SDK as our practical laboratory platform to learn hands-on **Object Storage** mechanics.

### Object Storage Concept Mapping

| Cloud Object Storage Concept | Cloudinary Implementation |
| :--- | :--- |
| **Bucket** | Folder |
| **Object** | File (`Image`, `CSV`, `PDF`, `JSON`) |
| **URL** | Secure HTTPS Resource URL |
| **Upload Pipeline** | Node.js SDK / CLI Commands |

---

### Step-by-Step Setup Guide

#### Step 1: Verify Node.js Environment
Ensure Node.js LTS is installed on your local computer:
```bash
node -v
npm -v

Step 2: Initialize Project & Install SDK
Open terminal and run:


mkdir cloudinary-demo
cd cloudinary-demo
npm init -y
npm install cloudinary dotenv
Step 3: Configure Credentials (.env)
Create a .env file in your root folder containing your Cloudinary credentials:


echo CLOUDINARY_CLOUD_NAME=your_cloud_name > .env
echo CLOUDINARY_API_KEY=your_api_key >> .env
echo CLOUDINARY_API_SECRET=your_api_secret >> .env
Step 4: Create Sample Data
Generate a dummy CSV file locally:


echo id,name,salary > employees.csv
echo 1,Rahul,45000 >> employees.csv
echo 2,Anita,52000 >> employees.csv
Step 5: Upload Raw Data (upload.js)
Create upload.js file:

JavaScript
const cloudinary = require("cloudinary").v2;
require("dotenv").config();

cloudinary.config({
  cloud_name: process.env.CLOUDINARY_CLOUD_NAME,
  api_key: process.env.CLOUDINARY_API_KEY,
  api_secret: process.env.CLOUDINARY_API_SECRET,
});

(async () => {
  try {
    const result = await cloudinary.uploader.upload("employees.csv", {
      resource_type: "raw",
      folder: "data_engineering"
    });
    console.log("Upload Successful");
    console.log("Public ID:", result.public_id);
    console.log("URL:", result.secure_url);
  } catch (err) {
    console.error("Upload Failed:", err.message);
  }
})();
Run upload script:


node upload.js

Expected Terminal Output:


Upload Successful
data_engineering/employees
[https://res.cloudinary.com/](https://res.cloudinary.com/)<your_cloud_name>/raw/upload/v123456789/data_engineering/employees.csv

Step 6: Batch Upload Media Files (uploadImages.js)
Create uploadImages.js file:

JavaScript
const cloudinary = require("cloudinary").v2;
require("dotenv").config();

cloudinary.config({
  cloud_name: process.env.CLOUDINARY_CLOUD_NAME,
  api_key: process.env.CLOUDINARY_API_KEY,
  api_secret: process.env.CLOUDINARY_API_SECRET,
});

async function uploadImages() {
  const images = [
    "C:/Users/raghu/Downloads/bmw.webp",
    "C:/Users/raghu/Downloads/bmw1.webp"
  ];

  for (const image of images) {
    try {
      const result = await cloudinary.uploader.upload(image, {
        folder: "cars",
      });
      console.log("Uploaded:", result.public_id);
      console.log("URL:", result.secure_url);
    } catch (err) {
      console.error("Error uploading", image);
      console.error(err.message);
    }
  }
}

uploadImages();

Run batch script:

node uploadImages.js

Cloudinary CLI Alternative Commands
You can also operate object storage directly from your terminal using Cloudinary CLI:

# Install CLI globally
npm install -g cloudinary-cli

# Check installed version
cloudinary --version

# Manual URL configuration (Windows Command Prompt)
set CLOUDINARY_URL=cloudinary://API_KEY:API_SECRET@CLOUD_NAME

# Verify URL variable
echo %CLOUDINARY_URL%

# Directly upload file
cloudinary upload employees.csv

# Upload directly to designated folder
cloudinary upload employees.csv -f department/hr

# List resources in bucket
cloudinary resources

# Delete an object
cloudinary delete employees
7. Hands-on Lab Assignment
Account Registration: Sign up for a free Cloudinary account and locate your Cloud Name, API Key, and API Secret on your dashboard.

Environment File: Securely place your credentials in a local .env file without committing it to version control.

Data Upload: Modify upload.js to upload two sample CSV datasets into a bucket folder named data_warehouse_raw.

Verification: Retrieve and open the generated HTTPS secure URLs inside your web browser to confirm accessibility.

CLI Operation: Practice listing bucket contents and removing an object using cloudinary resources and cloudinary delete terminal commands.