# Telecom Data Warehouse (DWH)
An ETL (SSIS) Project
## Project Overview
An ETL solution designed to process telecom transaction data received through CSV files every 5 minutes. The solution validates and transforms the data, loads valid records into SQL Server, handles rejected records separately, and archives processed files.
![Project Dataset Schema Overview](Pictures/Screenshots/Screenshot 2026-10-15 133928)
## Tools & Technologies
- SQL Server
- SQL Server Integration Services (SSIS)
- SQL (T-SQL)
- CSV Files

## ETL Workflow
1. **Extract:** Read incoming CSV files from the source folder.
2. **Transform:** Apply the required data transformations and look up subscriber information.
3. **Validate:** Check required fields and validate timestamp formats.
4. **Load:** Load valid records into the target SQL Server table.
5. **Reject:** Store invalid records in a separate table along with the original source filename.
6. **Archive:** Move successfully processed CSV files to the `Archive/Processed` folder.

## Data Transformation Rules

| Source Column | Transformation | Target Column |
|---|---|---|
| ID | As-is | Transaction_id |
| IMSI | Reject if null | IMSI |
| IMSI | Lookup subscriber_id; use -99999 if not found | subscriber_id |
| IMEI | Extract first 8 characters; use -99999 if null or shorter than 14 characters | TAC |
| IMEI | Extract last 6 characters; use -99999 if null or shorter than 14 characters | SNR |
| IMEI | As-is | IMEI |
| CELL | Reject if null | CELL |
| LAC | Reject if null | LAC |
| EVENT_TYPE | As-is | EVENT_TYPE |
| EVENT_TS | Validate and convert to datetime; reject if null or invalid | EVENT_TS |

## Rejected Records Handling
Invalid records are stored in a separate rejection table. The original CSV filename is also recorded to support tracking and auditing.

## File Archiving
After successful data loading, the processed CSV file is Copied(you can change it easily) to the `Archive/Processed` folder.

## Key Features
- Automated ETL workflow using SSIS
- Data validation and transformation
- Subscriber lookup using a reference table
- Rejected records management
- Source file tracking for auditing
- Automated file archiving

## Project Objective
To build a reliable and traceable ETL pipeline for processing recurring telecom transaction files and loading clean data into SQL Server.
