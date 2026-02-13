# Oracle Pluggable Database Management Assignment

**Course:** Database Development with PL/SQL (INSY 8311)  
**Student Name:** MUNYAWERA MURUNGI Henry Bernard  
**Student ID:** 28890  
**Submission Date:** February 2026  
**Instructor:** Eric Maniraguha

---

## Overview

This repository contains documentation and evidence for the Oracle Pluggable Database (PDB) Management assignment. The assignment demonstrates practical understanding of Oracle Multitenant Architecture, PDB creation and deletion, user management, and Oracle Enterprise Manager usage.

---

## Oracle Environment

- **Database:** Oracle Database 21c Express Edition
- **Version:** 21.3.0.0.0
- **Platform:** Microsoft Windows x86 64-bit
- **Container Database:** XE
- **Oracle Enterprise Manager:** Database Express

---

## Tasks Completed

### Task 1: Create a New Pluggable Database

**Objective:** Create a permanent PDB with proper naming conventions and user account.

**Steps Performed:**
1. Connected to CDB as SYSDBA
2. Created PDB with name: `he_pdb_28890`
3. Opened the PDB in READ WRITE mode
4. Switched to the PDB container
5. Created user: `henry_plsqlauca_28890`
6. Granted CONNECT, RESOURCE, and DBA privileges to the user
7. Verified user creation

**Key Commands:**
```sql
-- Connect as SYSDBA
CONNECT sys AS SYSDBA;

-- Create the Pluggable Database
CREATE PLUGGABLE DATABASE he_pdb_28890
ADMIN USER pdb_admin IDENTIFIED BY Pass123
CREATE_FILE_DEST = 'C:\LOCAL\ORADATA\XE';

-- Open the PDB
ALTER PLUGGABLE DATABASE he_pdb_28890 OPEN;

-- Verify PDB is open
SELECT name, open_mode FROM v$pdbs WHERE name = 'HE_PDB_28890';

-- Switch to the PDB
ALTER SESSION SET CONTAINER = he_pdb_28890;

-- Create user
CREATE USER henry_plsqlauca_28890 IDENTIFIED BY 1234;

-- Grant privileges
GRANT CONNECT, RESOURCE, DBA TO henry_plsqlauca_28890;

-- Verify user creation
SELECT username FROM dba_users WHERE username = 'HENRY_PLSQLAUCA_28890';
```

**Evidence:**
![PDB creation](https://github.com/user-attachments/assets/b3f8101e-5c42-4eee-879f-aa265c4ac1c1)

![PDB is in write mode ](https://github.com/user-attachments/assets/4f4e273f-4867-4b9b-ba0b-fa3a1a408a65)

![User Created ](https://github.com/user-attachments/assets/dd7d2be9-75ba-4671-8bcf-83cf2c203590)


---

### Task 2: Create and Delete a PDB

**Objective:** Create a temporary PDB and then delete it completely.

**Steps Performed:**
1. Connected to CDB root
2. Created temporary PDB: `he_to_delete_pdb_28890`
3. Verified PDB existence (MOUNTED state)
4. Dropped the PDB including all datafiles
5. Confirmed complete deletion

**Key Commands:**
```sql
-- Switch to CDB root
ALTER SESSION SET CONTAINER = CDB$ROOT;

-- Create temporary PDB
CREATE PLUGGABLE DATABASE he_to_delete_pdb_28890
ADMIN USER temp_admin IDENTIFIED BY Pass123
CREATE_FILE_DEST = 'C:\LOCAL\ORADATA\XE';

-- Verify PDB exists
SELECT name, open_mode FROM v$pdbs WHERE name = 'HE_TO_DELETE_PDB_28890';

-- Drop the PDB with datafiles
DROP PLUGGABLE DATABASE he_to_delete_pdb_28890 INCLUDING DATAFILES;

-- Confirm deletion
SELECT name, open_mode FROM v$pdbs WHERE name = 'HE_TO_DELETE_PDB_28890';
-- Result: no rows selected (PDB successfully deleted)
```
**Evidence:**

![Temporary PDB Creation](https://github.com/user-attachments/assets/ede234b1-90d3-4c30-8ff1-4b2456ad3f28)

![Temporary PDB existence verification](https://github.com/user-attachments/assets/2601548e-d5e1-4ea5-95b3-296d8c56d168)

![Temporary PDB deletion](https://github.com/user-attachments/assets/1bf7030f-9d0f-463a-8c6b-23467839918e)

![Temp PDB no longer exit ](https://github.com/user-attachments/assets/2f0845f8-dff5-4fe9-9e1d-126ae49ebb3b)


---

### Task 3: Oracle Enterprise Manager (OEM)

**Objective:** Access and document the Oracle Enterprise Manager dashboard.

**Steps Performed:**
1. Accessed OEM at https://localhost:5500/em
2. Logged in with SYSTEM credentials
3. Viewed Database Home dashboard
4. Verified CDB with 2 PDBs
5. Captured dashboard showing database status and resources

**Evidence:**
![OEM Dashboard ](https://github.com/user-attachments/assets/d8d6dac5-5e02-48e0-99e8-07c18676aa6d)

---

### Task 4: Documentation & Reporting

**Repository Structure:**
```
oracle_pdb_ass_II_28890_henry/
├── README.md (this file)
└── screenshots/
    ├── task1_pdb_creation.png
    ├── task1_pdb_open.png
    ├── task1_user_created.png
    ├── task2_temp_pdb_created.png
    ├── task2_temp_pdb_exists.png
    ├── task2_pdb_deleted.png
    ├── task2_deletion_confirmed.png
    └── task3_oem_dashboard.png
```

---

## Challenges Faced and Solutions

### Challenge 1: Initial Naming Error
**Problem:** Initially created PDB and user with incorrect naming convention (using "mu" prefix and "munyawera" instead of "he" prefix and "henry").

**Solution:** 
- Dropped the incorrectly named PDB (`mu_pdb_28890`) completely
- Recreated all components with correct naming conventions
- Re-executed all tasks with proper names: `he_pdb_28890` and `henry_plsqlauca_28890`
- Captured fresh screenshots with correct names

### Challenge 2: PDB File Path Error
**Problem:** First attempt to create PDB failed with ORA-65165 error due to incorrect file path specification.

**Solution:** 
- Queried v$datafile to identify the correct Oracle data directory path
- Discovered the correct path: `C:\LOCAL\ORADATA\XE`
- Used CREATE_FILE_DEST parameter with the correct path
- Successfully created PDB

### Challenge 3: Privilege and Container Context Issues
**Problem:** Encountered privilege errors when trying to grant privileges to user and "database not open" error after reconnecting.

**Solution:**
- Ensured proper connection as SYSDBA for administrative operations
- Always verified current container context using ALTER SESSION SET CONTAINER
- Reopened PDB when necessary after disconnection
- Maintained proper sequence: open PDB → switch to PDB → create/modify users

### Challenge 4: OEM SSL Certificate Warning
**Problem:** Browser showed security warning when accessing Oracle Enterprise Manager.

**Solution:**
- Proceeded through the security warning (expected behavior for self-signed certificates on localhost)
- Successfully accessed OEM dashboard
- Captured required dashboard screenshot showing database environment

---

## Key Learnings

1. **Oracle Multitenant Architecture:** Understanding the relationship between CDB (Container Database) and PDBs (Pluggable Databases)
2. **PDB Lifecycle Management:** Creating, opening, closing, and deleting pluggable databases
3. **Container Context Awareness:** Importance of knowing which container you're connected to (CDB$ROOT vs specific PDB)
4. **User Management in PDBs:** Creating users within specific PDB contexts with appropriate privileges
5. **Troubleshooting Skills:** Resolving path issues, privilege errors, and database connectivity problems
6. **Oracle Enterprise Manager:** Using OEM for database monitoring, management, and visualization
7. **Attention to Detail:** Following exact naming conventions as specified in assignment requirements

---

## Academic Integrity Statement

I, MUNYAWERA MURUNGI Henry Bernard 28890, declare that this assignment is my original work. All commands were executed by me individually, and all screenshots are from my own Oracle Database environment. I have not copied from classmates. This work reflects my own understanding and execution of Oracle Pluggable Database management tasks.

I encountered an initial naming error which I corrected by recreating all PDBs with the proper naming conventions based on my first name "Henry" as required by the assignment instructions.


---

## References

- Oracle Database 21c Documentation
- Oracle Multitenant Architecture Guide  
- Oracle Database Administrator's Guide
- Course materials: Database Development with PL/SQL (INSY 8311)
- Oracle Enterprise Manager Database Express User Guide

---

**End of Documentation**
