# Individual Assignment II: Oracle Pluggable Databases (PDB) Management

## Course Information
- **Course:** Database Development with PL/SQL (INSY 8311)
- **Student ID:** 29686
- **Student Name:** Melissa Nahimana

---

## Overview of Tasks
This assignment covers administration and operational management of Oracle Multitenant Architecture, including creating and opening Pluggable Databases (PDBs), provisioning local users with administrative grants, executing container teardown (dropping PDBs), and monitoring container activity via Oracle Enterprise Manager (OEM) Express.

---

## Oracle Environment Used
- **Database:** Oracle Database 21c Enterprise Edition Release 21.0.0.0.0
- **Operating System:** Microsoft Windows x86 64-bit
- **Tools:** SQL*Plus, Oracle SQL Developer, Oracle Enterprise Manager (OEM) Database Express

---

## Technical Task Documentation

### Task 1: Create a Permanent Pluggable Database & User
- **PDB Created:** `me_pdb_29686`
- **User Created:** `melissa_plsqlauca_29686`
- **Procedure:**
  1. Created the pluggable database from the seed using explicit file mappings:
     ```sql
     CREATE PLUGGABLE DATABASE me_pdb_29686 
     ADMIN USER pdb_admin IDENTIFIED BY Admin12345 
     FILE_NAME_CONVERT = ('C:\ORACLEAPP\ORADATA\ORCL\PDBSEED\', 'C:\ORACLEAPP\ORADATA\ORCL\ME_PDB_29686\');
     ```
  2. Opened the database in `READ WRITE` mode and preserved state across restarts using `ALTER PLUGGABLE DATABASE me_pdb_29686 SAVE STATE;`.
  3. Switched session context to container `me_pdb_29686`.
  4. Created user `melissa_plsqlauca_29686`, assigned permissions (`CONNECT`, `RESOURCE`, `DBA`), and granted `UNLIMITED TABLESPACE`.

#### Evidence:
- **PDB Creation:**
  ![PDB Creation](screenshots/pdb_creation/Screenshot%201%20PL&SQL.jpg)
- **PDB Open Mode:**
  ![PDB Open State](screenshots/pdb_creation/Screenshot%202%20PL&SQL.jpg)
- **User Verification:**
  ![User Created](screenshots/pdb_creation/Screenshot%203%20PL&SQL.jpg)

---


### Task 2: Create and Delete a Temporary PDB
- **Temporary PDB:** `me_to_delete_pdb_29686`
- **Procedure:**
  1. Created the container from `CDB$ROOT` using `FILE_NAME_CONVERT` to test lifecycle teardown procedures.
  2. Verified existence using `SHOW PDBS;`.
  3. Closed the database using `ALTER PLUGGABLE DATABASE me_to_delete_pdb_29686 CLOSE IMMEDIATE;`.
  4. Dropped the database along with its physical files using `DROP PLUGGABLE DATABASE me_to_delete_pdb_29686 INCLUDING DATAFILES;`.
  5. Verified removal using `SHOW PDBS;`.
#### Evidence:
- **Temporary PDB Creation:**
  ![Temp PDB Create](screenshots/pdb_deletion/Screenshot%204%20PL&SQL.jpg)
- **Temporary PDB Deletion:**
  ![Temp PDB Drop](screenshots/pdb_deletion/Screenshot%205%20PL&SQL.jpg)

---


### Task 3: Oracle Enterprise Manager (OEM) Setup
- **Procedure:**
  1. Configured HTTPS port 5500 on `CDB$ROOT` using `dbms_xdb_config.sethttpsport(5500)`.
  2. Accessed OEM Database Express via `https://localhost:5500/em`.
  3. Inspected the container overview dashboard confirming active status for container `ME_PDB_29686`.
#### Evidence:
- **OEM Dashboard Overview:**
  ![OEM Dashboard](screenshots/oem_dashboard/Screenshot%206%20PL&SQL.jpg)

---
## Challenges Faced & Solutions
1. **Error ORA-65016 (FILE_NAME_CONVERT must be specified):**
   - *Cause:* Oracle Managed Files (OMF) was not enabled by default.
   - *Solution:* Queried `v$datafile` to determine the seed path (`C:\ORACLEAPP\ORADATA\ORCL\PDBSEED\`) and explicitly provided source and target directory arguments via `FILE_NAME_CONVERT`.

2. **SQL Developer Connection Issue (ORA-12505):**
   - *Cause:* PDBs cannot connect via SID (`xe`).
   - *Solution:* Changed the connection setting to **Service name** with `me_pdb_29686` and registered the service with the listener using `ALTER SYSTEM REGISTER;`.

---


## Submission Details Block
Repository Link: https://github.com/Melissaaaaaaaaaa/oracle_pdb_ass_II_29686_melissa

PDB Name Created: me_pdb_29686

Issues Encountered: Yes
