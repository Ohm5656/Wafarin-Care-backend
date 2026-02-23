# Wafarin-Care-backend
Backend API for a hospital-grade Warfarin management platform—INR tracking, dose adjustment workflows, and secure provider–patient messaging with role-based access and audit logging.
## System Flow (Backend)

```mermaid
flowchart LR

C[Client Request] --> G0((AND))
G0 --> JWT[JWT Verify]
JWT --> OK{Valid}

OK -->|no| E1[401]
OK -->|yes| R0((AND))
R0 --> ROLE[Role Check]
ROLE --> PERM{Allowed}

PERM -->|no| E2[403]
PERM -->|yes| X0((OR))

%% Routes (เหมือนแตกแขนง Block 5/6/7 ในรูป)
X0 --> A1[Auth API]
X0 --> P1[Patients API]
X0 --> I1[INR API]
X0 --> M1[Medication API]
X0 --> AP1[Appointment API]
X0 --> S1[Safety API]
X0 --> AN1[Analytics API]
X0 --> N1[Notification API]

%% Auth
A1 --> D1[(users)]
A1 --> D2[(hospital_staff)]
A1 --> D3[(patients)]
A1 --> OUT1[Token and Profile]

%% Patients
P1 --> D3
P1 --> D4[(inr_records)]
P1 --> OUT2[Patient Data]

%% INR with status and notify
I1 --> D3
I1 --> CALC[Calc INR Status]
CALC --> D4
CALC --> K0((OR))
K0 -->|monitor| N2[Create Notification]
K0 -->|danger| N2
N2 --> D8[(notifications)]
I1 --> OUT3[INR Result]

%% Medication
M1 --> D5[(medication_logs)]
M1 --> OUT4[Medication Data]

%% Appointment
AP1 --> D6[(appointments)]
AP1 --> OUT5[Appointment Data]

%% Safety
S1 --> D7[(safety_items)]
S1 --> OUT6[Safety Data]

%% Analytics (อ่านหลายตาราง)
AN1 --> Q0((AND))
Q0 --> D3
Q0 --> D4
Q0 --> D5
AN1 --> OUT7[Analytics Data]

%% Notifications list
N1 --> D8
N1 --> OUT8[Notification List]

%% Cron jobs
CR[Scheduler Jobs] --> J0((OR))
J0 --> J1[Midnight Missed Update]
J0 --> J2[Morning Appointment Reminder]
J1 --> D5
J2 --> D6
J1 --> D8
J2 --> D8
```
