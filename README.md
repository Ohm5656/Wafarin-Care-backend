# Wafarin-Care-backend
Backend API for a hospital-grade Warfarin management platform—INR tracking, dose adjustment workflows, and secure provider–patient messaging with role-based access and audit logging.
## System Flow (Backend)

```mermaid
flowchart TB

REQ([Client Request]) --> GW[API Router]

GW --> AUTHN{JWT Valid}
AUTHN -->|No| R401[Unauthorized]
AUTHN -->|Yes| AUTHZ{Role Allowed}
AUTHZ -->|No| R403[Forbidden]

AUTHZ --> ROUTE{Endpoint}

ROUTE --> AUTH[Auth Service]
AUTH --> USERS[(users table)]
AUTH --> PROFILE{Role}
PROFILE --> STAFF[(hospital_staff)]
PROFILE --> PATIENT[(patients)]
AUTH --> RES_AUTH[Return Token and Profile]

ROUTE --> PATMOD[Patients Service]
PATMOD --> PATIENT
PATMOD --> INR[(inr_records)]
PATMOD --> RES_PAT[Return Patient Data]

ROUTE --> INRMOD[INR Service]
INRMOD --> PATIENT
INRMOD --> CALC[Calculate INR Status]
CALC --> INR
CALC --> NOTI[Create Notification]
NOTI --> NOTITBL[(notifications)]
INRMOD --> RES_INR[Return INR Record]

ROUTE --> MEDMOD[Medication Service]
MEDMOD --> MED[(medication_logs)]
MEDMOD --> RES_MED[Return Medication Data]

ROUTE --> APMOD[Appointment Service]
APMOD --> APPT[(appointments)]
APMOD --> RES_APPT[Return Appointment Data]

ROUTE --> SAFEMOD[Safety Service]
SAFEMOD --> SAFE[(safety_items)]
SAFEMOD --> RES_SAFE[Return Safety Data]

ROUTE --> ANALYTICS[Analytics Service]
ANALYTICS --> PATIENT
ANALYTICS --> INR
ANALYTICS --> MED
ANALYTICS --> RES_ANL[Return Analytics Data]

CRON[Scheduler Jobs] --> MED
CRON --> APPT
CRON --> NOTITBL
```
