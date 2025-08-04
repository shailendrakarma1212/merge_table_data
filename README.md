# 📘 Merge_Ope_Table Notebook — Patient Data Merge (2025-08-04)

## 📌 Overview

This Databricks notebook demonstrates how to **create**, **insert**, and **merge** patient data using Delta Lake. It is useful for healthcare use-cases where patient data is stored in multiple sources and needs to be combined while avoiding duplicates.

---

## 📂 Tables Used

### 1. `merge_table.patient`
- Stores raw patient data with appointment information.
- Created with a Delta Lake table format.

**Columns:**
| Column           | Type    | Description              |
|------------------|---------|--------------------------|
| patient_id       | INT     | Unique ID for each patient |
| patient_name     | STRING  | Full name of patient     |
| age              | INT     | Age of patient           |
| gender           | STRING  | Gender of patient        |
| doctor_name      | STRING  | Name of attending doctor |
| appointment_date | DATE    | Date of appointment      |

---

### 2. `merge_table.totle_merge_patient`
- Target table that stores **merged** patient records.
- Ensures data integrity (no duplicate `patient_id`s).

---

## 🔧 Operations Performed

### ✅ Create Tables
```sql
CREATE TABLE IF NOT EXISTS my_unity_catalog_metastore.merge_table.patient (...)
CREATE TABLE IF NOT EXISTS my_unity_catalog_metastore.merge_table.totle_merge_patient (...)
```

### 📝 Insert Records
- Inserts initial patient records into `patient`.
- Later inserts new rows into `totle_merge_patient`.

### 🔁 Merge Operation
```sql
MERGE INTO totle_merge_patient AS target
USING patient AS source
ON target.patient_id = source.patient_id
WHEN NOT MATCHED THEN
  INSERT (...)
```
- Ensures only **new patient records** are added to `totle_merge_patient`.

---

## 📊 Sample Inserted Data

```sql
(1, 'Alice Smith', 30, 'Female', 'Dr. Brown', '2025-08-01'),
(2, 'Bob Johnson', 45, 'Male', 'Dr. Green', '2025-08-02'),
(3, 'Carol Lee', 28, 'Female', 'Dr. White', '2025-08-03')
```

Additional patients merged:

```sql
(4, 'David Roy', 36, 'Male', 'Dr. Mehra', '2025-08-05'),
(5, 'Emma Khan', 29, 'Female', 'Dr. Sharma', '2025-08-06'),
(6, 'Ravi Patel', 50, 'Male', 'Dr. Gupta', '2025-08-07')
```

---

## 🎯 Purpose of Merge

- **Prevent duplicate records**.
- **Combine patient & appointment data**.
- **Automate healthcare record keeping** using Delta Lake's `MERGE INTO`.

---

## ✅ How to Use

1. Run the cell to create the source and target tables.
2. Insert new records into `patient`.
3. Use the `MERGE` SQL command to populate `totle_merge_patient` safely.
4. Query `totle_merge_patient` to confirm data.
