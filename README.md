# Seattle Pet License Dimensional Modeling - (Staging Data)

## Project Overview
The **Seattle Pet License Dimensional Modeling - Part 1** project focuses on setting up a **staging environment** to store and preprocess pet license data from **Seattle's pet registration records**. The data is loaded into a **SQL Server or MySQL database** using **Talend ETL workflows**, ensuring an efficient and scalable **data warehousing solution**.

This project involves:

- **Creating a stage table** with appropriate data types and schema.
- **Developing Talend jobs** to extract, transform, and load (ETL) data into the stage table.
- **Implementing metadata and context variables** for a dynamic, maintainable ETL workflow.
- **Ensuring no hardcoded values** in Talend components.
- **Using repository-based connections** instead of built-in methods for database connections.

---

## Project Objectives

**Create a Stage Table** in **SQL Server or MySQL** with an optimized schema.  
**Design & Implement Talend ETL Jobs** to load data into the stage table.  
**Use Metadata & Context Variables** to manage **multiple environments** (Dev, Test, Prod).  
**Eliminate Hardcoded Values** and ensure reusable ETL workflows.  
**Utilize Repository-Based Connections** for secure and efficient data integration.  

---

## Files in this Repository

| File Name | Description |
|-----------|------------|
| `Seattle_Pet_License_Ansh_Vaghela.pdf` | Contains Talend workflow screenshots, SQL queries, and staging table details |
| `Seattle_Pet_License_Stage_Table.sql` | SQL script to create the stage table |
| `Talend_Job.tJob` | Talend job file for loading data |
| `README.md` | Project documentation |

---

## Dataset Overview

The **Seattle Pet License Dataset** consists of records of **pet registrations** in Seattle, including pet type, breed, zip code, and license details.

### **Stage Table Schema**

The **Stage Table** is structured as follows:

| Column Name        | Data Type    | Description |
|--------------------|-------------|-------------|
| `License_Number`   | VARCHAR(50)  | Unique identifier for pet license |
| `Issue_Date`       | DATE         | Date license was issued |
| `Expiration_Date`  | DATE         | License expiration date |
| `Species`         | VARCHAR(50)  | Type of pet (Dog, Cat, etc.) |
| `Primary_Breed`    | VARCHAR(100) | Primary breed of the pet |
| `Secondary_Breed`  | VARCHAR(100) | Secondary breed if applicable |
| `Zip_Code`        | VARCHAR(10)  | ZIP code of pet owner |
| `DI_CreatedDate`   | DATETIME     | Metadata column (Record creation timestamp) |
| `DI_ProcessID`     | VARCHAR(50)  | Metadata column (ETL Process Identifier) |

**Mandatory Schema Columns:**

- **`DI_CreatedDate`** - Captures the timestamp of when the record was loaded.
- **`DI_ProcessID`** - Stores a unique process ID to track ETL jobs.

---

## ETL Process using Talend

### Talend Job Design  
The **Talend ETL job** extracts data from the **TSV file**, transforms it, and loads it into the **stage table**.

### Key Features:
- **Uses Metadata for Schema Management**  
- **Implements Context Variables** for different environments (**Dev, Test, Prod**)  
- **Eliminates Hardcoded Values** to improve reusability  
- **Repository-Based Connection Method** for secure data management  

---

### Talend Context Variables  
The **Talend job** is configured with **three environments**:

| Environment | Database Connection | File Path |
|------------|---------------------|-----------|
| **Dev** | `jdbc:mysql://dev_server/seattle_pet` | `/data/dev/Seattle_Pet_License.tsv` |
| **Test** | `jdbc:mysql://test_server/seattle_pet` | `/data/test/Seattle_Pet_License.tsv` |
| **Prod** | `jdbc:mysql://prod_server/seattle_pet` | `/data/prod/Seattle_Pet_License.tsv` |

---

### Context Variables Used:
- **`DB_Host`** → Stores the database host (**changes per environment**).  
- **`DB_Name`** → Stores the database name.  
- **`DB_User`** → Stores the database username.  
- **`DB_Password`** → Stores the database password (**secured**).  
- **`File_Path`** → Stores the **input file path dynamically** for each environment.  

---

**Using these configurations, the ETL job ensures flexibility, security, and reusability across different environments.** 🚀


## **SQL Script to Create Stage Table**
```sql
CREATE TABLE Seattle_Pet_License_Stage (
    License_Number VARCHAR(50) PRIMARY KEY,
    Issue_Date DATE,
    Expiration_Date DATE,
    Species VARCHAR(50),
    Primary_Breed VARCHAR(100),
    Secondary_Breed VARCHAR(100),
    Zip_Code VARCHAR(10),
    DI_CreatedDate DATETIME DEFAULT CURRENT_TIMESTAMP,
    DI_ProcessID VARCHAR(50) NOT NULL
);
```
--

## Data Validation & Testing

After loading data into the **stage table**, validation steps include:

### Row Count Verification:
```sql
SELECT COUNT(*) FROM Seattle_Pet_License_Stage;
```

### Check for Duplicates:
```sql
SELECT License_Number, COUNT(*) 
FROM Seattle_Pet_License_Stage 
GROUP BY License_Number 
HAVING COUNT(*) > 1;
```

### Ensure Proper Data Types:
```sql
SELECT COLUMN_NAME, DATA_TYPE 
FROM INFORMATION_SCHEMA.COLUMNS 
WHERE TABLE_NAME = 'Seattle_Pet_License_Stage';
```
