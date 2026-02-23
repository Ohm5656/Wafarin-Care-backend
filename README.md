# Wafarin-Care-backend
Backend API for a hospital-grade Warfarin management platform—INR tracking, dose adjustment workflows, and secure provider–patient messaging with role-based access and audit logging.
## System Flow (Backend) – Styled

```mermaid
flowchart TB

%% ===== Nodes =====
client([Client Frontend])
router[API Router]

authn{JWT Valid}
authz{Role Allowed}

r401[Unauthorized]
r403[Forbidden]

%% ===== Sections =====
subgraph AUTH[Auth]
  login[Login]
  users[(users)]
  staff[(hospital_staff)]
  patient[(patients)]
  token[Token and Profile]
end

subgraph CORE[Core APIs]
  pats[Patients API]
  inrsvc[INR API]
  medsvc[Medication API]
  apptsvc[Appointment API]
  safesvc[Safety API]
  anlsvc[Analytics API]
end

subgraph DATA[Database Tables]
  inr[(inr_records)]
  meds[(medication_logs)]
  appt[(appointments)]
  safe[(safety_items)]
  noti[(notifications)]
end

subgraph JOBS[Scheduler Jobs]
  cron[Daily Jobs]
  job1[Midnight Update Missed Meds]
  job2[Morning Appointment Reminder]
end

subgraph NOTIF[Notifications]
  notisvc[Notification Service]
end

%% ===== Flow =====
client --> router --> authn
authn -->|no| r401
authn -->|yes| authz
authz -->|no| r403
authz -->|yes| login

login --> users
login --> staff
login --> patient
login --> token

router --> pats --> patient
pats --> inr
pats --> token

router --> inrsvc --> patient
inrsvc --> inr
inrsvc --> notisvc --> noti

router --> medsvc --> meds
router --> apptsvc --> appt
router --> safesvc --> safe
router --> anlsvc --> patient
anlsvc --> inr
anlsvc --> meds

cron --> job1 --> meds
cron --> job2 --> appt
job1 --> noti
job2 --> noti

%% ===== Styling =====
classDef main fill:#0F6B3A,color:#ffffff,stroke:#0B4F2B,stroke-width:2px;
classDef sub fill:#E8F5EC,color:#0F6B3A,stroke:#1E8449,stroke-width:1.5px;
classDef db fill:#ffffff,color:#0F6B3A,stroke:#1E8449,stroke-width:2px;
classDef warn fill:#FDEDEC,color:#922B21,stroke:#C0392B,stroke-width:2px;
classDef job fill:#FEF9E7,color:#7D6608,stroke:#B7950B,stroke-width:2px;

class router,login,notisvc main
class AUTH,CORE,DATA,JOBS,NOTIF sub
class users,staff,patient,inr,meds,appt,safe,noti db
class r401,r403 warn
class cron,job1,job2 job
```
