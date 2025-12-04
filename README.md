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
**Next Phase:** Logical database design (ERD + Data Dictionary)

























## 📋 Delivery Requirements

**Course Details:**  
- **Course:** Database Development with PL/SQL (INSY 8311)  
- **Academic Year:** 2025-2026 | Semester: I  
- **Institution:** Adventist University of Central Africa (AUCA)  
- **Project Completion Date:** December 7, 2025


