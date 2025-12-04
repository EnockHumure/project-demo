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

**Phase:** III - Business Process Modeling  
**Focus:** Workflow design with analytics prioritization  


# Phase III: Logical Database Design

## 📊 Entity-Relationship Model

### **Entities and Descriptions**

| Entity | Purpose | Key Attributes |
|--------|---------|----------------|
| **reception** | Stores patient demographic information collected at registration | `patient_id` (PK), `first_name`, `last_name`, `gender`, `date_of_birth`, `phone_number`, `disease_name` |
| **doctor** | Stores doctor information for diagnosis and treatment tracking | `doctor_id` (PK), `first_name`, `last_name`, `specialization` |
| **lab_technician** | Records laboratory test results for patients | `lab_test_id` (PK), `patient_id` (FK), `test_type`, `test_result`, `test_date`, `lab_technician_name` |
| **treatment** | Tracks medication and treatment provided to patients | `treatment_id` (PK), `patient_id` (FK), `medication`, `dosage`, `doctor_id` (FK), `date_given` |
| **disease_stats** | Fact table for disease analytics and BI reporting | `stats_id` (PK), `disease_name` (FK), `total_cases`, `new_cases`, `date_recorded` |
| **main_diseases** | Stores the 5 priority diseases for dashboard analytics | `disease_id` (PK), `disease_name` |
| **other_diseases** | Stores diseases not in the main priority list | `other_disease_id` (PK), `disease_name`, `description` |

### **Priority Diseases List**
- Malaria
- HIV/AIDS
- Stunting
- Respiratory Infections
- Diarrheal Diseases

## 🔗 Relationships & Cardinalities

```
1. One patient (reception) → Many lab tests (lab_technician) [1:N]
2. One patient (reception) → Many treatments (treatment) [1:N]
3. One disease (main_diseases) → Many statistics (disease_stats) [1:N]
4. Doctors can prescribe multiple treatments [1:N]
5. Diseases classified as either main or other (exclusive)
```

## 🏗️ Database Normalization

### **Third Normal Form (3NF) Compliance**
- **1NF:** All tables have atomic values, no repeating groups
- **2NF:** All non-key attributes fully dependent on primary keys
- **3NF:** No transitive dependencies; non-key attributes depend only on PK

### **Normalization Example:**
```
Original denormalized: reception(patient_id, disease_name, disease_description)
Normalized to:
  - reception(patient_id, disease_name) [disease_name as FK reference]
  - main_diseases(disease_id, disease_name, description) OR
  - other_diseases(other_disease_id, disease_name, description)
```

## 📋 Data Dictionary

| Table | Description | Primary Key | Foreign Keys |
|-------|-------------|-------------|--------------|
| **reception** | Patient demographic and registration data | `patient_id` | - |
| **doctor** | Healthcare provider information | `doctor_id` | - |
| **lab_technician** | Laboratory test results and metadata | `lab_test_id` | `patient_id` → reception |
| **treatment** | Medication and treatment records | `treatment_id` | `patient_id` → reception, `doctor_id` → doctor |
| **disease_stats** | Aggregated disease metrics for analytics | `stats_id` | `disease_name` → main_diseases |
| **main_diseases** | Priority disease definitions | `disease_id` | - |
| **other_diseases** | Non-priority disease definitions | `other_disease_id` | - |

## 📈 BI-Optimized Schema Design

### **Fact Table:**
- **disease_stats** - Central analytics table tracking disease metrics over time

### **Dimension Tables:**
- **reception** - Patient dimension
- **doctor** - Provider dimension  
- **lab_technician** - Test dimension
- **treatment** - Treatment dimension
- **main_diseases** - Disease classification dimension

### **Analytics Ready Features:**
- Star schema design for efficient queries
- Pre-aggregated metrics in `disease_stats`
- Time-series data for trend analysis
- Dimensional hierarchies for drill-down reporting

## 🎯 ER Diagram (Text-Based)

```
    +----------------+           +----------------+           +----------------+
    |   reception    |1         *|  lab_technician|*         1|     doctor     |
    +----------------+-----------+----------------+-----------+----------------+
    | patient_id PK  |           | lab_test_id PK |           | doctor_id PK   |
    | first_name     |           | patient_id FK  |           | first_name     |
    | last_name      |           | test_type      |           | last_name      |
    | gender         |           | test_result    |           | specialization |
    | date_of_birth  |           | test_date      |           +----------------+
    | phone_number   |           | lab_technician_name|      
    | disease_name   |           +----------------+
    +----------------+       

           |1
           |
           * 
    +----------------+
    |    treatment   |
    +----------------+
    | treatment_id PK|
    | patient_id FK  |
    | medication     |
    | dosage         |
    | doctor_id FK   |
    | date_given     |
    +----------------+

           |
           * 
    +----------------+
    |  disease_stats |
    +----------------+
    | stats_id PK    |
    | disease_name FK|
    | total_cases    |
    | new_cases      |
    | date_recorded  |
    +----------------+

           |
           * 
    +----------------+          +----------------+
    | main_diseases  |          | other_diseases |
    +----------------+          +----------------+
    | disease_id PK  |          | other_disease_id PK |
    | disease_name   |          | disease_name        |
    +----------------+          | description         |
                                +----------------+
```

## 🔄 Data Flow Architecture

```
Patient Registration → Disease Classification → Clinical Process → Analytics
      (reception)          (main/other)      (doctor/lab/treatment)  (disease_stats)
```

## 🛡️ Data Integrity Constraints

### **Primary Keys:**
- Unique identifiers for all entities
- Auto-increment sequences for surrogate keys

### **Foreign Keys:**
- Enforce referential integrity between related tables
- Cascade updates for consistency
- Restrict deletes to prevent orphan records

### **Check Constraints:**
- Valid gender values (M/F/O)
- Date ranges (no future dates for birth/visit)
- Positive values for numeric fields

## 📊 Design Justification

### **Why Separate Main/Other Diseases?**
1. **Analytics Focus:** Prioritize resources on major public health concerns
2. **Performance:** Efficient queries for priority diseases
3. **Scalability:** Easy to modify disease priority lists
4. **Clarity:** Clear separation for reporting purposes

### **Why disease_stats Fact Table?**
1. **BI Optimization:** Pre-aggregated metrics for fast dashboards
2. **Historical Tracking:** Time-series disease trends
3. **Reduced Query Complexity:** Simplified analytics queries
4. **Consistency:** Single source of truth for disease metrics

---

**Phase:** III - Logical Database Design  
**Status:** 3NF Compliant, BI-Optimized  
























## 📋 Delivery Requirements

**Course Details:**  
- **Course:** Database Development with PL/SQL (INSY 8311)  
- **Academic Year:** 2025-2026 | Semester: I  
- **Institution:** Adventist University of Central Africa (AUCA)  
- **Project Completion Date:** December 7, 2025


