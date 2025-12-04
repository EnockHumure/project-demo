# Patient Disease Tracking & Analytics System (PDTAS)

=============================================================
## 🎓 Personal Information

**Student:** Humure Enock  
**ID:** 27394  
**Program:** AUCA - IT - Software Engineering  
**Course:** INSY 8311 | Database Development with PL/SQL  
**Faculty:** Information Technology - AUCA  
**Lecturer:** Eric Maniraguha  
**Group:** Wednesday(C)  
**Project Title:** Patient Disease Tracking & Analytics System (PDTAS)

## 🏥 Project Overview

This is a multi-phase individual capstone project centered on Oracle database design, PL/SQL development, and Business Intelligence implementation. The project serves as the FINAL EXAM and significantly impacts the course grade.

## 🔍 Problem Analysis

**Current Challenge:** Healthcare providers lack a unified system to track patient flows and disease-specific outcomes across reception, clinical, and lab departments. This makes it hard to monitor disease incidence, patient follow-ups, and resource allocation for Malaria, HIV/AIDS, Stunting, Respiratory Infections, and Diarrheal Diseases.

**Research Question:** Can we predict workplace injury patterns? (Note: This appears to be from a different project - maintaining original text)

## 🛠 Solution Architecture

**System Solution:** A PL/SQL-backed Patient Tracking System that records patient registration, clinical encounters, diagnostics and treatments and produces BI-ready data for disease surveillance and decision-making.

## 🏛 Implementation Context

**Usage Environment:** Used in hospital outpatient departments and clinics to capture patient identity, visit lifecycle (reception → clinician → lab → treatment → follow-up) and disease-specific details.

**Target User Groups:**
- Receptionists
- Doctors  
- Lab Technicians
- Pharmacists
- Hospital Managers
- Public Health Analysts

## 🎯 Project Objectives

**Primary Goals:**
- Accurate patient tracking
- Audit trail of all actions
- Disease incidence reports
- KPI dashboards (incidence rates, treatment outcomes, no-show rates)

## 📊 Business Intelligence Potential

**Analytics Capabilities:**
- Dashboards for disease trends
- Hotspots identification
- Age/sex distributions analysis
- Treatment success monitoring
- Resource usage optimization

## 📚 Development Framework

### **Project Phases table of content **

| Phase | Primary Objective | Key Deliverable |
|-------|-------------------|-----------------|
| I | Problem Identification | PowerPoint Presentation |
| II | Business Process Modeling | UML/BPMN Diagram |
| III | Logical Database Design | ER Diagram + Data Dictionary |
| IV | Database Creation | Oracle PDB + Configuration |
| V | Table Implementation | CREATE/INSERT Scripts |
| VI | PL/SQL Development | Procedures, Functions, Packages |
| VII | Advanced Programming | Triggers, Auditing, Security |
| VIII | Final Documentation | GitHub Repo + Presentation |


# Phase II: Business Process Modeling

## 📋 Business Process Overview
The Patient Disease Tracking & Analytics System follows a structured workflow from patient arrival to analytics generation, with special focus on disease classification for prioritized analytics.

## 👥 System Actors
- **Receptionist** - Registers patients and captures initial disease information
- **Nurse/Triage** - Performs initial assessment and vital checks
- **Doctor** - Provides diagnosis, orders tests, and prescribes treatment
- **Lab Technician** - Conducts and records test results
- **Pharmacist** - Dispenses medications
- **Health Information Manager** - Generates analytics and reports

## 🔄 Core Process Flow

### **Step 1: Patient Registration & Disease Classification**
- Patient arrives at facility
- Receptionist registers patient or looks up existing record
- **Critical Decision:** Receptionist asks about primary disease/symptoms
  - **Main Disease Path:** If disease is in priority list (Malaria, HIV/AIDS, Stunting, Respiratory Infections, Diarrheal Diseases) → data routed to `disease_stats` table for dashboard analytics
  - **Other Disease Path:** If disease is not in priority list → data stored in `other_diseases` table

### **Step 2: Clinical Assessment & Treatment**
- **Nurse/Triage:** Records vital signs and triage information (optional)
- **Doctor:** Confirms diagnosis, orders tests, prescribes treatment
- **Important:** All patients receive full treatment regardless of disease classification
- **Lab Technician:** Performs ordered tests and records results
- **Pharmacist:** Dispenses prescribed medications

### **Step 3: Analytics & Reporting**
- **Health Information Manager:** Generates analytics with two-tier approach:
  - **Priority Analytics:** Main diseases tracked in real-time dashboards with alerts
  - **Secondary Analytics:** Other diseases included in periodic reports
- System maintains audit logs for all operations
- Business rules enforced (no operations on weekdays/holidays - Phase VII)



## 📊 Disease Classification Impact
| **Main Diseases** | **Other Diseases** |
|-------------------|-------------------|
| Stored in `disease_stats` table | Stored in `other_diseases` table |
| Priority in real-time dashboards | Included in standard reports |
| Trigger public health alerts | No alert generation |
| Focus of resource allocation | Standard care tracking |

## ⚠️ Key Process Rules
1. **Treatment Equality:** All patients receive complete clinical care
2. **Classification Decision:** Made at reception, confirmed by doctor
3. **Analytics Priority:** Only main diseases get real-time dashboard updates
4. **Data Integrity:** All diseases recorded, analytics priority differs

## 🔗 Process Output
- Complete patient treatment records for all cases
- Prioritized analytics for main diseases
- Comprehensive data for public health monitoring
- Audit trail of all system activities

---

**Phase:** II - Business Process Modeling  
**Focus:** Workflow design with analytics prioritization  


# Phase III: Logical Model Design

## 🎯 Objective
Design a detailed 3NF-compliant logical data model for the Patient Disease Tracking & Analytics System, ensuring data integrity and BI readiness.

## 📊 Entity-Relationship Model

### **Entities (7 Tables)**
| Entity | Description | PK | Key Attributes |
|--------|-------------|----|----------------|
| `reception` | Patient registration data | `patient_id` | Demographic info + disease classification |
| `doctor` | Healthcare provider details | `doctor_id` | Doctor information + specialization |
| `lab_technician` | Laboratory test results | `lab_test_id` | Test types, results, dates |
| `treatment` | Medication administration | `treatment_id` | Medication, dosage, prescribing doctor |
| `disease_stats` | Disease analytics fact table | `stats_id` | Case counts, trends, dates |
| `main_diseases` | Priority diseases (5) | `disease_id` | Malaria, HIV/AIDS, Stunting, etc. |
| `other_diseases` | Non-priority diseases | `other_disease_id` | Other conditions |

### **Cardinalities**
```
reception (1) → (*) lab_technician
reception (1) → (*) treatment
doctor (1) → (*) treatment
main_diseases (1) → (*) disease_stats
```


## 🎯 ER Diagram (Mermaid Format)

```mermaid
erDiagram
    RECEPTION ||--o{ LAB_TECHNICIAN : "receives_tests"
    RECEPTION ||--o{ TREATMENT : "receives_treatment"
    DOCTOR ||--o{ TREATMENT : "prescribes"
    MAIN_DISEASES ||--o{ DISEASE_STATS : "tracked_in"
    RECEPTION }o--|| MAIN_DISEASES : "classified_as_main"
    RECEPTION }o--|| OTHER_DISEASES : "classified_as_other"
```

### **Constraints**
- **PK/FK relationships** maintain referential integrity
- **Check constraints** for data validation
- **NOT NULL** for mandatory fields
- **UNIQUE** for critical identifiers

## 🏗️ Normalization (3NF Compliance)

### **1NF - Atomic Values**
- Each table cell contains single values
- No repeating groups or arrays
- Example: `medication` stored as separate text, not comma-separated list

### **2NF - No Partial Dependencies**
- All non-key attributes depend on entire primary key
- Example: In `treatment`, all attributes depend on `treatment_id` (not just `patient_id`)

### **3NF - No Transitive Dependencies**
- Non-key attributes depend only on primary key
- Example: Disease description moved to disease tables, not duplicated in `reception`

## 📋 Data Dictionary

| Table | PK | FK | Key Columns | Data Types |
|-------|----|----|-------------|------------|
| reception | patient_id | - | first_name, gender, disease_name | VARCHAR2, DATE, VARCHAR2 |
| doctor | doctor_id | - | specialization, last_name | VARCHAR2 |
| lab_technician | lab_test_id | patient_id | test_type, test_result | VARCHAR2, VARCHAR2 |
| treatment | treatment_id | patient_id, doctor_id | medication, dosage, date_given | VARCHAR2, VARCHAR2, DATE |
| disease_stats | stats_id | disease_name | total_cases, date_recorded | NUMBER, DATE |
| main_diseases | disease_id | - | disease_name | VARCHAR2 |
| other_diseases | other_disease_id | - | disease_name, description | VARCHAR2, VARCHAR2 |

## 📈 BI Considerations

### **Fact vs Dimension Tables**
- **Fact Table:** `disease_stats` (measures: total_cases, new_cases)
- **Dimension Tables:** `reception`, `doctor`, `lab_technician`, `treatment`, `main_diseases`

### **Slowly Changing Dimensions**
- **Type 1 (Overwrite):** Doctor specialization changes
- **Type 2 (Historical):** Patient disease classification changes
- **Type 3 (Limited History):** Disease priority status changes

### **Aggregation Levels**
- **Daily:** New case counts
- **Weekly/Monthly:** Trend analysis
- **Yearly:** Public health reporting
- **Roll-up:** Disease → Category → System-wide

### **Audit Trail Design**
- Separate `audit_log` table (Phase VII)
- Track: user, action, table, record_id, timestamp
- Support rollback and compliance reporting

## 🔧 Design Assumptions

1. **Data Volume:** 100-500 patients per main table
2. **Concurrency:** Multiple concurrent users (reception, doctors, lab)
3. **Retention:** 5+ years of historical data
4. **Performance:** Sub-second response for dashboard queries
5. **Security:** Role-based access control (Phase VII)


## 📊 Schema Summary

```
TOTAL TABLES: 7
TOTAL RELATIONSHIPS: 6
NORMALIZATION LEVEL: 3NF
BI READINESS: Star schema implemented
AUDIT READINESS: Audit trail framework defined
```





---

**Phase:** III - Logical Model Design  
**Status:** ✅ Completed  
**Compliance:** 3NF + BI Optimized  

# Phase IV: Database Creation

## 🎯 Objective
Create and configure the Oracle pluggable database for the Patient Disease Tracking & Analytics System with proper tablespace management and user setup.

## 📁 Database Setup

### **Naming Convention (As Required)**
- **Group:** Wednesday (WED)
- **Student ID:** 27394
- **First Name:** Enock
- **Project Name:** Patient Disease Tracking & Analytics System (PDTAS)

**Final PDB Name:** `WED_27394_ENOCK_PDTAS_DB`

### **Administrative Configuration**
| Component | Value | Purpose |
|-----------|-------|---------|
| **Admin Username** | `enock_admin` | Super administrator for the PDB |
| **Admin Password** | `humure` | Student's first name as required |
| **Admin Privileges** | DBA role | Full administrative control |
| **Application User** | `patient_track` | Application-level database user |
| **Application Password** | `humure` | Same as admin for simplicity |

## ⚙️ Database Configuration

### **Tablespace Configuration**
| Tablespace | Type | Size | Autoextend | Purpose |
|------------|------|------|------------|---------|
| `pdta_data` | Data | 50MB | ON (Next 10M, Max 500M) | Stores all table data |
| `pdta_index` | Index | 20MB | ON (Next 5M, Max 200M) | Stores indexes for performance |
| `pdta_temp` | Temporary | 20MB | ON (Next 5M, Max 100M) | Temporary operations space |

### **Memory Parameters**
| Parameter | Value | Justification |
|-----------|-------|---------------|
| **SGA** | Oracle XE Default | Limited by Oracle Express Edition |
| **PGA** | Oracle XE Default | Limited by Oracle Express Edition |
| **Total Memory** | As per XE limits | 2GB total for Oracle XE 21c |

**Note:** Oracle Express Edition has memory limitations. Production deployment would require tuning.

### **Archive Logging Status**
```
STATUS: DISABLED (Oracle XE Limitation)
REASON: Oracle Express Edition does not support archive logging
WORKAROUND: Regular backups via RMAN or data export
```

### **Autoextend Parameters**
- **All tablespaces:** AUTOEXTEND ON
- **Data growth:** Managed with NEXT and MAXSIZE parameters
- **Monitoring:** Regular space usage checks required

## 📜 SQL Implementation Scripts

### **1. Database Creation Script (`phase4_create_pdb.sql`)**
```sql
-- Phase IV: Database Creation for PDTAS
-- Student: Humure Enock (ID: 27394)
-- Group: Wednesday (C)
-- Date: December 2024

-- Step 1: Create Pluggable Database
CREATE PLUGGABLE DATABASE WED_27394_ENOCK_PDTAS_DB
  ADMIN USER enock_admin IDENTIFIED BY humure
  ROLES = (DBA)
  FILE_NAME_CONVERT = (
    'C:\dbms_oracle\oradata\XE\pdbseed\',
    'C:\dbms_oracle\oradata\XE\WED_27394_ENOCK_PDTAS_DB\'
  );

-- Step 2: Open PDB
ALTER PLUGGABLE DATABASE WED_27394_ENOCK_PDTAS_DB OPEN;
ALTER PLUGGABLE DATABASE WED_27394_ENOCK_PDTAS_DB SAVE STATE;

-- Step 3: Switch to PDB
ALTER SESSION SET CONTAINER = WED_27394_ENOCK_PDTAS_DB;

-- Step 4: Create Tablespaces
CREATE TABLESPACE pdta_data 
DATAFILE 'C:\dbms_oracle\oradata\XE\WED_27394_ENOCK_PDTAS_DB\pdta_data01.dbf'
SIZE 50M AUTOEXTEND ON NEXT 10M MAXSIZE 500M;

CREATE TABLESPACE pdta_index
DATAFILE 'C:\dbms_oracle\oradata\XE\WED_27394_ENOCK_PDTAS_DB\pdta_index01.dbf'
SIZE 20M AUTOEXTEND ON NEXT 5M MAXSIZE 200M;

CREATE TEMPORARY TABLESPACE pdta_temp
TEMPFILE 'C:\dbms_oracle\oradata\XE\WED_27394_ENOCK_PDTAS_DB\pdta_temp01.dbf'
SIZE 20M AUTOEXTEND ON NEXT 5M MAXSIZE 100M;

-- Step 5: Create Application User
CREATE USER patient_track IDENTIFIED BY humure
DEFAULT TABLESPACE pdta_data
TEMPORARY TABLESPACE pdta_temp
QUOTA UNLIMITED ON pdta_data;

GRANT CONNECT, RESOURCE, DBA TO patient_track;

-- Verification Queries
SELECT name, open_mode FROM v$pdbs;
SELECT tablespace_name, status FROM dba_tablespaces;
SELECT username, account_status FROM dba_users;
```

### **2. Verification Script (`phase4_verify.sql`)**
```sql
-- Verify PDB Status
SELECT name, open_mode, con_id FROM v$pdbs WHERE name = 'WED_27394_ENOCK_PDTAS_DB';

-- Verify Tablespaces
SELECT tablespace_name, status, contents, extent_management 
FROM dba_tablespaces 
WHERE tablespace_name LIKE 'PDTA%';

-- Verify Users
SELECT username, account_status, default_tablespace, temporary_tablespace
FROM dba_users 
WHERE username IN ('ENOCK_ADMIN', 'PATIENT_TRACK');

-- Verify Datafiles
SELECT file_name, tablespace_name, bytes/1024/1024 as size_mb, autoextensible
FROM dba_data_files 
WHERE tablespace_name LIKE 'PDTA%';
```

## 👥 User Setup Documentation

### **Administrative User (`enock_admin`)**
```sql
-- Privileges: Full DBA rights
-- Purpose: Database administration, user management, backup/restore
-- Security: Strong password required in production
```

### **Application User (`patient_track`)**
```sql
-- Privileges: CONNECT, RESOURCE, DBA (for development)
-- Purpose: Application data operations (Phase V-VII)
-- Default Tablespace: pdta_data
-- Temporary Tablespace: pdta_temp
```


## 🔒 Security Considerations

### **Production Recommendations:**
1. **Password Policy:** Strong passwords (12+ chars, mixed case, numbers, symbols)
2. **Role Separation:** Application user should not have DBA in production
3. **Audit Trail:** Enable auditing for admin activities
4. **Regular Rotation:** Password rotation every 90 days

### **Development Settings:**
- DBA granted for development flexibility
- Simple passwords for ease of testing
- Local environment only (not exposed to network)

## 📊 Performance Considerations

### **Tablespace Strategy:**
- **Separation:** Data and indexes in separate tablespaces for I/O optimization
- **Autoextend:** Prevents out-of-space errors during data loading
- **Sizing:** Initial sizes based on estimated Phase V data volume (100-500 records per table)

### **Future Scalability:**
1. **Add Tablespaces:** Separate tablespaces for different table types if needed
2. **Partitioning:** Consider partitioning for large tables (>1M rows)
3. **Compression:** Enable table compression for historical data

## ✅ Verification Checklist

### **Pre-Creation:**
- [ ] Oracle XE 21c installed and running
- [ ] SYSDBA access available
- [ ] Sufficient disk space (minimum 1GB free)
- [ ] Backup of existing databases if any

### **Post-Creation Verification:**
- [ ] PDB created and in OPEN state
- [ ] All tablespaces created successfully
- [ ] Users created with correct privileges
- [ ] Quotas assigned properly
- [ ] Connection possible with new users

### **Documentation Complete:**
- [ ] SQL scripts saved to GitHub
- [ ] Screenshots captured
- [ ] README.md updated
- [ ] All configuration decisions documented

## ⚠️ Known Limitations & Workarounds

| Limitation | Reason | Workaround |
|------------|--------|------------|
| No archive logging | Oracle XE restriction | Regular RMAN backups |
| Memory limits | XE 2GB total limit | Optimize SGA/PGA ratios |
| No partitioning | XE feature restriction | Manual data archiving |
| Single PDB | XE allows 3 PDBs total | Manage PDB count carefully |

## 🚀 Next Phase Preparation

### **Ready for Phase V:**
1. **Database:** PDB created and configured
2. **Users:** Application user with necessary privileges
3. **Tablespaces:** Optimized for table creation and data loading
4. **Documentation:** Complete for submission

### **Connection Details for Phase V:**
```
Host: localhost
Port: 1521
Service: XE
PDB: WED_27394_ENOCK_PDTAS_DB
User: patient_track
Password: humure
```

---

**Phase:** IV - Database Creation  



# Phase V: Table Implementation & Data Verification

## 🎯 Objective
Verify the successful creation of all database tables, data integrity, and proper implementation of the Patient Disease Tracking & Analytics System schema.

## 📋 Table Structure Verification

### **Tables Created (7 Total):**
| Table Name | Purpose | Row Count |
|------------|---------|-----------|
| `MAIN_DISEASES` | Priority disease definitions | 5 |
| `OTHER_DISEASES` | Non-priority disease definitions | 3 |
| `RECEPTION` | Patient registration data | 5 |
| `DOCTOR` | Healthcare provider information | 5 |
| `LAB_TECHNICIAN` | Laboratory test records | 5 |
| `TREATMENT` | Medication and treatment history | 5 |
| `DISEASE_STATS` | Analytics and disease metrics | 5 |

## 🔍 Validation Script Results

### **1. Table Existence Check**
```sql
SELECT table_name 
FROM user_tables
WHERE table_name IN (
    'MAIN_DISEASES',
    'OTHER_DISEASES',
    'RECEPTION',
    'DOCTOR',
    'LAB_TECHNICIAN',
    'TREATMENT',
    'DISEASE_STATS'
);
```
**Expected Result:** All 7 tables should be listed.

### **2. Column Structure Verification**
```sql
SELECT column_name, data_type, nullable
FROM user_tab_columns
WHERE table_name = 'RECEPTION';
```
**Expected Result:** Should show columns: `patient_id`, `first_name`, `last_name`, `gender`, `date_of_birth`, `phone_number`, `email`, `address`, `disease_name`, `visit_date`, `doctor_id`, `lab_technician_id`, `treatment_id`.

### **3. Constraint Validation**
```sql
SELECT constraint_name, constraint_type, table_name
FROM user_constraints
WHERE table_name IN (
    'RECEPTION',
    'LAB_TECHNICIAN',
    'TREATMENT',
    'DOCTOR',
    'DISEASE_STATS'
);
```
**Expected Result:** Should show PRIMARY KEY and FOREIGN KEY constraints for each table.

### **4. Data Volume Verification**
```sql
SELECT COUNT(*) AS main_diseases_count FROM main_diseases;        -- Expected: 5
SELECT COUNT(*) AS other_diseases_count FROM other_diseases;     -- Expected: 3
SELECT COUNT(*) AS reception_count FROM reception;               -- Expected: 5
SELECT COUNT(*) AS doctor_count FROM doctor;                     -- Expected: 5
SELECT COUNT(*) AS lab_technician_count FROM lab_technician;     -- Expected: 5
SELECT COUNT(*) AS treatment_count FROM treatment;               -- Expected: 5
SELECT COUNT(*) AS disease_stats_count FROM disease_stats;       -- Expected: 5
```

## 🔗 Foreign Key Integrity Tests

### **1. Orphan Lab Tests Check**
```sql
SELECT *
FROM lab_technician l
WHERE NOT EXISTS (
    SELECT 1 FROM reception r WHERE r.patient_id = l.patient_id
);
```
**Expected Result:** 0 rows (no orphan records)

### **2. Orphan Treatment Records Check**
```sql
SELECT *
FROM treatment t
WHERE NOT EXISTS (
    SELECT 1 FROM reception r WHERE r.patient_id = t.patient_id
);
```
**Expected Result:** 0 rows (no orphan records)

## 📊 Sample Data Queries

### **1. Patient Data Retrieval**
```sql
-- View all patients
SELECT * FROM reception;

-- Expected: 5 rows with Rwandan patient names and diseases
```

### **2. Lab Tests with Patient Information**
```sql
SELECT r.first_name, r.last_name, l.test_type, l.test_result, l.lab_technician_name
FROM reception r
JOIN lab_technician l ON r.patient_id = l.patient_id;

-- Expected: 5 rows showing patient names with their lab test results
```

### **3. Treatment Records with Patient Information**
```sql
SELECT r.first_name, r.last_name, t.medication, t.dosage, t.pharmacist_name
FROM reception r
JOIN treatment t ON r.patient_id = t.patient_id;

-- Expected: 5 rows showing prescribed medications for each patient
```

## 📈 Analytics Queries

### **1. Disease Distribution Analysis**
```sql
SELECT disease_name, COUNT(*) AS patient_count
FROM reception
GROUP BY disease_name;

-- Expected: 5 rows, one for each main disease with patient counts
```

### **2. Hospital Location Statistics**
```sql
SELECT hospital_location, disease_name, SUM(patient_count) AS total_patients
FROM disease_stats
GROUP BY hospital_location, disease_name;

-- Expected: Shows disease statistics by hospital location
```

## 🔍 Edge Case Tests

### **1. Patients Without Lab Tests**
```sql
SELECT first_name, last_name
FROM reception r
WHERE patient_id IN (SELECT patient_id FROM treatment)
  AND patient_id NOT IN (SELECT patient_id FROM lab_technician);

-- Expected: Should identify any inconsistencies in data flow
```

### **2. Disease Classification Validation**
```sql
SELECT r.first_name, r.last_name, r.disease_name
FROM reception r
WHERE r.disease_name NOT IN (SELECT disease_name FROM main_diseases);

-- Expected: Should return 0 rows (all diseases should be in main_diseases)
-- Note: This assumes all patients have main diseases. For other diseases, this query would need adjustment.
```

## 📊 Dashboard-Ready Queries

### **Top Diseases by Patient Count**
```sql
SELECT disease_name, COUNT(*) AS patient_count
FROM reception
GROUP BY disease_name
ORDER BY patient_count DESC;

-- Expected: List of main diseases sorted by number of patients
```

## ✅ Validation Summary

### **Pass/Fail Criteria:**
- [ ] **Table Creation:** All 7 tables exist
- [ ] **Data Insertion:** All tables have sample data (5 rows each minimum)
- [ ] **Foreign Keys:** No orphan records found
- [ ] **Basic Queries:** All SELECT queries return expected results
- [ ] **JOIN Operations:** All joins work correctly
- [ ] **Aggregations:** GROUP BY queries calculate correctly
- [ ] **Subqueries:** Complex queries execute without errors

### **Execution Instructions:**
1. Connect to the PDB: `WED_27394_ENOCK_PDTAS_DB`
2. Run as user: `patient_track`
3. Execute the validation script in SQL Developer or SQL*Plus
4. Verify all expected results match

## 📁 Files for Submission

### **Phase V Scripts:**
- `phase5_create_tables.sql` - Table creation script
- `phase5_insert_data.sql` - Data insertion script
- `phase5_validation.sql` - This validation script
- `README.md` - This documentation

### **Expected Output Files:**
- Screenshot of table creation success
- Screenshot of data insertion results
- Screenshot of validation script execution
- Screenshot of sample query results

---

**Phase:** V - Table Implementation & Data Insertion  
**Status:** ✅ Completed  
**Database:** WED_27394_ENOCK_PDTAS_DB  
**User:** patient_track  
**Next Phase:** VI - PL/SQL Development



















## 📋 Delivery Requirements

**Course Details:**  
- **Course:** Database Development with PL/SQL (INSY 8311)  
- **Academic Year:** 2025-2026 | Semester: I  
- **Institution:** Adventist University of Central Africa (AUCA)  
- **Project Completion Date:** December 7, 2025


