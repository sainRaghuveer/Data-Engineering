# Database Management Systems: SQL (MS SQL Server) vs NoSQL (MongoDB)

This repository provides a comprehensive guide to working with Relational Databases (SQL) and Non-Relational Databases (NoSQL). It covers installation, core terminology, concepts, and detailed command-line interface (CLI) and graphical user interface (GUI) operations for both **MS SQL Server** and **MongoDB**.

---

## Table of Contents
1. [Core Terminology & Concept Mapping](#1-core-terminology--concept-mapping)
2. [Installation Guide](#2-installation-guide)
3. [Relational Database Management System: MS SQL Server](#3-relational-database-management-system-ms-sql-server)
   * [GUI Operations (SSMS)](#gui-operations-ssms)
   * [CLI Operations (sqlcmd / T-SQL)](#cli-operations-sqlcmd--t-sql)
4. [NoSQL Database Management System: MongoDB](#4-nosql-database-management-system-mongodb)
   * [GUI Operations (MongoDB Compass)](#gui-operations-mongodb-compass)
   * [CLI Operations (mongosh)](#cli-operations-mongosh)
5. [Summary Comparison Chart](#5-summary-comparison-chart)

---

## 1. Core Terminology & Concept Mapping

| SQL / Relational Concept (MS SQL) | NoSQL / Document Concept (MongoDB) | Definition |
| :--- | :--- | :--- |
| **Database** | **Database** | Container for storing related tables/collections. |
| **Table** | **Collection** | Structure holding data records. |
| **Row / Record** | **Document** | Single entry containing data points. |
| **Column / Field** | **Field / Property** | Attribute or key within a data entry. |
| **Schema** | **Dynamic Schema** | Structural definition governing attributes and data types. |
| **Primary Key** | **Primary Key (`_id`)** | Unique identifier for a given row or document. |
| **Foreign Key** | **Reference / Embedded Doc** | Link between different data structures. |

---

## 2. Installation Guide

### MS SQL Server & SSMS
1. **MS SQL Server Express:** Download and install MS SQL Server Express from the official Microsoft site. Select standard installation options.
2. **SQL Server Management Studio (SSMS):** Download and install SSMS to manage SQL Server via GUI.
3. **Command Line Tools:** `sqlcmd` is included with SQL Server Command Line Utilities for CLI execution.

### MongoDB & MongoDB Compass
1. **MongoDB Community Server:** Download and install MongoDB Community Edition as a Windows/Linux service.
2. **MongoDB Shell (`mongosh`):** Download and install `mongosh` separately to execute database commands via CLI.
3. **MongoDB Compass:** Install MongoDB Compass (GUI) during server setup or standalone installer.

---

## 3. Relational Database Management System: MS SQL Server

MS SQL Server is a relational database management system (RDBMS) that stores data in structured tables and uses T-SQL (Transact-SQL) for data operations.

### GUI Operations (SSMS)
* **Create Database:** Right-click **Databases** in Object Explorer -> Select **New Database...** -> Enter name -> Click **OK**.
* **Create Table:** Expand your Database -> Right-click **Tables** -> Select **New** -> **Table...** -> Define column names, data types, and primary key -> Click **Save**.
* **Insert Data:** Right-click table -> Select **Edit Top 200 Rows** -> Manually type record values into cells.
* **Query Data:** Right-click table -> Select **Select Top 1000 Rows** or click **New Query** to write custom queries.
* **Drop Database/Table:** Right-click the object in Object Explorer -> Select **Delete** -> Click **OK**.

---

### CLI Operations (sqlcmd / T-SQL)

#### Connect to Server via CLI
```bash
sqlcmd -S localhost -U sa -P YourPassword

1. Database Operations

-- Create Database
CREATE DATABASE UniversityDB;
GO

-- Switch Database
USE UniversityDB;
GO

-- Drop Database
DROP DATABASE UniversityDB;
GO


2. DDL (Data Definition Language)

-- Create Table
CREATE TABLE Students (
    StudentID INT PRIMARY KEY IDENTITY(1,1),
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    Age INT,
    GPA DECIMAL(3,2),
    EnrollmentDate DATE DEFAULT GETDATE()
);
GO

-- Alter Table (Add Column)
ALTER TABLE Students ADD Email VARCHAR(100);
GO

-- Drop Table
DROP TABLE Students;
GO


3. DML (Data Manipulation Language)
Insert Records (Create)

-- Single Insert
INSERT INTO Students (FirstName, LastName, Age, GPA)
VALUES ('Rahul', 'Sharma', 21, 3.75);
GO

-- Multiple Insert
INSERT INTO Students (FirstName, LastName, Age, GPA)
VALUES 
('Anita', 'Verma', 22, 3.90),
('Vikram', 'Singh', 20, 3.40);
GO


Query Records (Read)

-- Select All
SELECT * FROM Students;
GO

-- Select with Filtering
SELECT FirstName, GPA 
FROM Students 
WHERE GPA >= 3.50 AND Age > 20;
GO

-- Sorting Data
SELECT * FROM Students 
ORDER BY GPA DESC;
GO


Update Records (Update)

-- Update specific record
UPDATE Students 
SET GPA = 3.85, Email = 'rahul@example.com' 
WHERE StudentID = 1;
GO


Delete Records (Delete)

-- Delete filtered record
DELETE FROM Students 
WHERE StudentID = 3;
GO

-- Truncate Table (Delete all rows)
TRUNCATE TABLE Students;
GO



## 4. NoSQL Database Management System: MongoDB

MongoDB is a document-oriented NoSQL database that stores data in flexible, JSON-like BSON (Binary JSON) documents.

---

### GUI Operations (MongoDB Compass)

* **Connect to Instance:** Launch Compass $\rightarrow$ Paste connection string `mongodb://localhost:27017` $\rightarrow$ Click **Connect**.
* **Create Database & Collection:** Click **Create database** $\rightarrow$ Enter Database Name and Collection Name $\rightarrow$ Click **Create Database**.
* **Insert Document:** Open collection $\rightarrow$ Click **Add Data** $\rightarrow$ **Insert Document** $\rightarrow$ Paste JSON data $\rightarrow$ Click **Insert**.
* **Query Documents:** Use the Filter bar (e.g., `{ "age": { "$gte": 21 } }`) $\rightarrow$ Click **Find**.
* **Drop Database/Collection:** Hover over Database or Collection in sidebar $\rightarrow$ Click the trash icon $\rightarrow$ Confirm deletion.

---

### CLI Operations (mongosh)

#### Connect to Server via CLI

```bash
mongosh "mongodb://localhost:27017"


1. Database Operations

// Show Databases
show dbs

// Create / Switch Database
use UniversityDB

// Check Current Database
db

// Drop Current Database
db.dropDatabase()



2. Collection Operations

// Show Collections
show collections

// Create Collection Explicitly
db.createCollection("students")

// Drop Collection
db.students.drop()



3. CRUD Operations
Insert Documents (Create)

// Insert Single Document
db.students.insertOne({
  firstName: "Rahul",
  lastName: "Sharma",
  age: 21,
  gpa: 3.75,
  skills: ["JavaScript", "MongoDB"]
})

// Insert Multiple Documents
db.students.insertMany([
  { firstName: "Anita", lastName: "Verma", age: 22, gpa: 3.90, skills: ["Python", "SQL"] },
  { firstName: "Vikram", lastName: "Singh", age: 20, gpa: 3.40, skills: ["Java", "C++"] }
])



Find Documents (Read)

// Find All Documents
db.students.find()

// Find with Formatting
db.students.find().pretty()

// Equality Filter
db.students.find({ firstName: "Rahul" })

// Comparison Operators ($gt,$gte, $lt,$lte)
db.students.find({ gpa: { $gte: 3.50 } })

// Logical Operators ($and,$or)
db.students.find({
  $or: [
    { age: { $lt: 21 } },     { gpa: {$gt: 3.80 } }
  ]
})

// Projection (Return specific fields)
db.students.find({}, { firstName: 1, gpa: 1, _id: 0 })



Update Documents (Update)

// Update One Document
db.students.updateOne(
  { firstName: "Rahul" },
  { 
    $set: { gpa: 3.85, email: "rahul@example.com" },
    $push: { skills: "Express.js" }
  }
)

// Update Multiple Documents
db.students.updateMany(
  { age: { $gte: 21 } },   {$set: { status: "Senior" } }
)



Delete Documents (Delete)

// Delete One Document
db.students.deleteOne({ firstName: "Vikram" })

// Delete Multiple Documents
db.students.deleteMany({ age: { $lt: 21 } })

// Clear Collection Documents
db.students.deleteMany({})






