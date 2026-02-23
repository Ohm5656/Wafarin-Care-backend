# Wafarin-Care-backend
Backend API for a hospital-grade Warfarin management platform—INR tracking, dose adjustment workflows, and secure provider–patient messaging with role-based access and audit logging.
## System Flow (Backend)

```mermaid
flowchart TB

REQ([Client Request]) --> GW[API Gateway / Router]

GW --> AUTHN{JWT Token?}
AUTHN -->|No| R401[401 Unauthorized]
AUTHN -->|Yes| AUTHZ{Role Allowed?}
AUTHZ -->|No| R403[403 Forbidden]

AUTHZ --> ROUTE{Which API?}

%% =========================
%% AUTH
%% =========================
ROUTE -->|/api/auth/*| AUTH[Auth Service]
AUTH --> U[(users)]
AUTH -->|Load profile| PROF{role?}
PROF -->|hospital_staff| S[(hospital_staff)]
PROF -->|patient| P[(patients)]
AUTH --> RES_AUTH[Return token + user + profile]

%% =========================
%% PATIENTS
%% =========================
ROUTE -->|/api/patients*| PATMOD[Patients Service]
PATMOD --> P
PATMOD --> R[(inr_records)]
PATMOD --> RES_PAT[Return patient(s) + latest INR/status]

%% =========================
%% INR RECORDS
%% =========================
ROUTE -->|/api/inr-records*| INRMOD[INR Service]
INRMOD --> P
INRMOD --> CALC[Calculate INR Status\nsafe/monitor/danger]
CALC --> R
CALC -->|monitor/danger| NOTI[Create Notification]
NOTI --> N[(notifications)]
INRMOD --> RES_INR[Return INR record + status]

%% =========================
%% MEDICATION LOGS
%% =========================
ROUTE -->|/api/medication-logs*| MEDMOD[Medication Service]
MEDMOD --> M[(medication_logs)]
MEDMOD --> RES_MED[Return schedule / confirm result]

%% =========================
%% APPOINTMENTS
%% =========================
ROUTE -->|/api/appointments*| APMOD[Appointments Service]
APMOD --> A[(appointments)]
APMOD --> RES_APPT[Return appointments]

%% =========================
%% SAFETY ITEMS (Public Search + Staff CRUD)
%% =========================
ROUTE -->|/api/safety-items*| SAFEMOD[Safety Service]
SAFEMOD --> SF[(safety_items)]
SAFEMOD --> RES_SAFE[Return safety items]

%% =========================
%% ANALYTICS (Staff Only)
%% =========================
ROUTE -->|/api/analytics*| ANLMOD[Analytics Service]
ANLMOD --> P
ANLMOD --> R
ANLMOD --> M
ANLMOD --> RES_ANL[Return KPIs + charts data]

%% =========================
%% NOTIFICATIONS (Polling / Optional Realtime)
%% =========================
ROUTE -->|/api/notifications*| NOTIMOD[Notifications Service]
NOTIMOD --> N
NOTIMOD --> RES_NOTI[Return notifications list]

%% =========================
%% CRON / SCHEDULER (Background Jobs)
%% =========================
CRON[Scheduler / Cron Jobs] --> C1[00:00 Update missed meds]
C1 --> M
C1 -->|Create reminder| N

CRON --> C2[09:00 Appointment reminder (T-1 day)]
C2 --> A
C2 -->|Create reminder| N
C2 -->|Mark reminder_sent=true| A
```
