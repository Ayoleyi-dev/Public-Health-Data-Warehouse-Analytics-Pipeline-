 Public Health Data Warehouse & Analytics Pipeline 🏥

<img width="1050" height="540" alt="image" src="https://github.com/user-attachments/assets/85fab478-9019-4bf0-ac72-14e234a610c5" />


## 📋 Table of Contents
- [Executive Summary](#executive-summary)
- [Business Problem](#business-problem)
- [Solution Architecture](#solution-architecture)
- [Data Engineering Logic](#data-engineering-logic)
- [Key Insights & Visualizations](#key-insights--visualizations)
- [Biochemistry Context](#biochemistry-context)
- [Setup & Usage](#setup--usage)

-----

## Executive Summary

This project demonstrates the design and implementation of a full-stack data analytics solution for a high-volume healthcare environment. By simulating a clinical dataset of 200+ patient records, I built a robust **Star Schema** data warehouse in **SQL Server** and connected it to **Power BI** for real-time disease tracking and financial reporting.

**Key Achievement:** Identified **Influenza** as the primary revenue driver (due to volume) and detected a correlation between **Patient Age** and **Case Severity**.

-----

## Business Problem

A mock public health institution needed a centralized system to:

1.  Track patient demographics, diagnoses, and hospital admissions.
2.  Analyze financial performance across different disease categories.
3.  Monitor public health trends (outbreaks, seasonality, and severity) to optimize resource allocation.

-----

## Solution Architecture

I designed a relational database using the **Star Schema** methodology to ensure query efficiency and referential integrity.

### 🛠️ Tech Stack

  * **Database:** Microsoft SQL Server (T-SQL)
  * **ETL & Scripting:** Stored Procedures, Dynamic SQL
  * **Visualization:** Power BI Desktop
  * **Version Control:** Git & GitHub

### 🗺️ Entity Relationship Diagram (ERD)

<img width="965" height="551" alt="image" src="https://github.com/user-attachments/assets/153f74de-5470-46fb-8e5b-2c1d9af74a51" />


  * **Fact Table:** `Consultations` (Transactions, Costs, Dates)
  * **Dimension Tables:** `Patients`, `Staff`, `Diseases`

-----

## Data Engineering Logic

To stress-test the database, I developed custom T-SQL scripts to generate realistic synthetic data.

**1. Automated Patient Intake Tool (`sp_AdmitPatient`)**
I built a Stored Procedure to standardize data entry, preventing human error and ensuring data types (like `DECIMAL` for weight) are strictly enforced.

**2. Dynamic Data Generation (ETL)**
Instead of static inserts, I used SQL loops and randomization functions to populate the warehouse:

  * **Randomization:** Used `FLOOR(RAND(CHECKSUM(NEWID())))` to generate varied age and weight distributions.
  * **Logic:** Used `CHOOSE()` to randomly assign names and cities from a predefined list, simulating diverse patient demographics.
  * **Data Cleaning:** Implemented `UPDATE` scripts to handle null values and backfill missing demographic data for legacy records.
### 🔧 Challenges & Resolutions
**Issue:** The synthetic data generator initially created null values due to an index mismatch in the `CHOOSE()` function.
**Resolution:** I implemented a dynamic sizing logic to ensure the random index always matched the array length.

**Code Snippet (Data Generation Logic):**
```sql
-- Dynamic Patient Generation Loop
WHILE @Counter <= 100
BEGIN
    INSERT INTO Patients ([Age], [Weight])
    VALUES (
        FLOOR(RAND(CHECKSUM(NEWID())) * 85) + 1, -- Generates Age 1-85
        CAST((RAND(CHECKSUM(NEWID())) * 68) + 2 AS DECIMAL(5,2)) -- Generates Weight
    );
    SET @Counter = @Counter + 1;
END;
-----
```
## Key Insights & Visualizations

**Dashboard Tool:** Power BI

**1. Financial Analysis**

  * **Finding:** While **Cholera** and **Lassa Fever** are "Critical" and expensive to treat, **Influenza** generates the highest total revenue due to a 3x higher admission rate.
  * **Action:** Hospital resources should be optimized for high-turnover, low-severity cases during peak flu seasons.

**2. Public Health Trends**

  * **Finding:** **January** showed a spike in admissions (Seasonality), suggesting a post-holiday surge in viral transmissions.
  * **Finding:** Average patient age increases with disease severity (Critical cases are skewed towards 60+ years old), highlighting the need for specialized geriatric care units.

-----

## Biochemistry Context

As a Biochemistry undergraduate, I designed this system to mirror the architecture used in **Bioinformatics LIMS (Laboratory Information Management Systems)**. The logic remains identical, mapping clinical entities to biological ones:

| Hospital Entity (Business) | Bioinformatics Entity (Science) | Purpose |
| :--- | :--- | :--- |
| **`Patients` Table** | **`Samples` Table** | Tracks the unique biological source (e.g., Cell Line, Tissue ID). |
| **`Staff` Table** | **`Researchers` Table** | Tracks the PI or Lab Technician responsible for the assay. |
| **`Diseases` Table** | **`Gene Variants` Table** | Stores reference data (e.g., Gene Name, Chromosome Location). |
| **`Consultations` Table** | **`Assays` Table** | The central fact table recording experimental results and quality metrics. |

-----

## Setup & Usage

To run this project locally:

1.  **Clone the repo:** `git clone https://github.com/YourUsername/Healthcare-Analytics.git`
2.  **Initialize Database:** Open `Healthcare_Script.sql` in SSMS and execute the setup script to create the schema and populate the data.
3.  **Launch Dashboard:** Open `Healthcare_Dashboard.pbix` in Power BI Desktop.
4.  **Connect:** Update the Data Source settings in Power BI to point to your local SQL Server instance (`localhost`).

-----

