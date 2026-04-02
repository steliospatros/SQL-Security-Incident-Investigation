# Security Incident Investigation via SQL Data Filtering

## 1. Project Overview
This project demonstrates the application of SQL queries to filter, analyze, and retrieve critical security data from a relational database. The objective was to investigate potential security incidents and identify specific employee devices requiring immediate security patches. 

The tasks involved querying connection logs and employee records to isolate anomalous activities, such as after-hours failed logins and out-of-scope geographical access attempts.

## 2. Tools & Environment
* **Database Management System:** MariaDB
* **Language:** SQL
* **Techniques:** Data filtering (WHERE clause), Logical operators (AND, OR, NOT), Pattern matching (LIKE, % wildcard).

## 3. Security Scenarios & Query Execution

### Scenario A: Investigating After-Hours Failed Login Attempts
To identify potential brute-force attacks or unauthorized access attempts occurring outside standard business hours, the log_in_attempts table was filtered for unsuccessful events after 18:00.

```sql
-- Retrieve all failed login attempts occurring post-18:00
SELECT * FROM log_in_attempts 
WHERE login_time > '18:00' AND success = FALSE;
```

### Scenario B: Tracking Anomalous Geographic Login Activity
Login attempts originating outside expected operational regions (Mexico) were isolated for further incident response investigation.

```sql
-- Filter login logs to exclude traffic originating from Mexico
-- Uses the LIKE operator with a wildcard to account for data format variations (MEX / MEXICO)
SELECT * FROM log_in_attempts 
WHERE NOT country LIKE 'MEX%';
```

### Scenario C: Identifying Target Machines for Security Patching
Specific organizational departments required critical workstation updates. Queries were designed to accurately pull employee and device information based on department and physical location parameters.

```sql
-- Isolate employee machines in the Marketing department located in the East building
SELECT * FROM employees 
WHERE department = 'Marketing' AND office LIKE 'East%';

-- Retrieve all employees outside of the IT department for standard patch rollout
SELECT * FROM employees 
WHERE NOT department = 'Information Technology';
```

## 4. Key Takeaways
This project highlights the ability to efficiently parse large datasets to extract actionable security intelligence. The strategic use of SQL logical operators ensures precise threat hunting and streamlined IT asset management workflows.
