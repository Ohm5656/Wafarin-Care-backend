# 🛡️ WafarinCare BACKEND

### Smart Warfarin Monitoring & AI-Assisted Safety Engine

> FastAPI-powered medical logic system
> Doctor-Verified • AI-Assisted • Real-Time • Production-Ready

---

# 🧠 Overview

WafarinCare Backend is the core medical engine behind the Coaguard platform.
It manages Warfarin patient monitoring, INR evaluation, medication tracking, appointment reminders, and AI-assisted food safety analysis.

This backend is designed with a **Safety-First Architecture**:

Doctor Input → Verified Database → Rule Engine → AI Interpretation → Safe Output

AI does NOT make medical decisions.
All final outcomes are grounded in doctor-defined rules.

---

# ⚙️ Tech Stack

![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge\&logo=fastapi\&logoColor=fff)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge\&logo=python\&logoColor=fff)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge\&logo=postgresql\&logoColor=fff)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-CA4245?style=for-the-badge)
![Alembic](https://img.shields.io/badge/Alembic-111827?style=for-the-badge)
![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge\&logo=jsonwebtokens\&logoColor=fff)
![BCrypt](https://img.shields.io/badge/Bcrypt-0B4F2B?style=for-the-badge)
![SSE](https://img.shields.io/badge/Realtime-SSE-2563EB?style=for-the-badge)
![Firebase](https://img.shields.io/badge/Push-FCM-FFCA28?style=for-the-badge\&logo=firebase\&logoColor=000)
![APScheduler](https://img.shields.io/badge/Cron-APScheduler-1F2937?style=for-the-badge)

---

# 🏗 System Architecture

## 🔐 Authentication Layer

* JWT-based authentication
* Role-based access control (RBAC)
* Password hashing with bcrypt
* Access token validation middleware

## 🩸 INR Monitoring Engine

* Automatic INR status calculation:

  * Safe (2.0 – 3.0)
  * Monitor
  * Danger
* Auto-trigger notifications on abnormal values
* Staff-controlled INR records

## 💊 Medication Management

* 30-day medication schedule generation
* Daily patient confirmation
* Midnight cron job to mark missed doses
* Adherence rate calculation

## 📅 Appointment System

* Appointment CRUD
* Reminder scheduling
* Auto notification trigger

## 🔔 Notification System

### Level 1 – Database Notification

* Notification table
* Read/unread status
* Priority levels (low → critical)

### Level 2 – Real-Time Notification

* Server-Sent Events (SSE)
* Instant dashboard updates
* Staff receives danger alerts immediately

### Level 3 – Web Push Notification

* Firebase Cloud Messaging (FCM)
* Device token storage
* Push alerts for:

  * Danger INR
  * Missed medication
  * Upcoming appointments

---

# 🍽 AI-Assisted Food Safety Engine

Patients enter a meal name.

Backend flow:

1. AI extracts ingredients from text
2. System matches against doctor-defined restricted items
3. Rule engine determines final status
4. AI summarizes result (grounded only in DB)
5. Returns Safe / Warning / Danger

No hallucinated medical advice.
All outputs are doctor-verified.

---

# 📡 API Structure

```
/api/auth
/api/patients
/api/inr-records
/api/medication-logs
/api/appointments
/api/notifications
/api/safety-items
/api/food-check
```

---

# 🗄 Database Schema

Core Tables:

* Users
* HospitalStaff
* Patients
* INRRecords
* MedicationLogs
* Appointments
* SafetyItems
* Notifications
* UserDevices

Relational integrity enforced via foreign keys.

---

# ⏱ Background Jobs (APScheduler)

* 00:00 → Update missed medication
* 09:00 → Appointment reminder generation
* Immediate → Danger INR push alert

---

# 🔒 Security Principles

* JWT authentication
* Bcrypt password hashing
* Role-based endpoint protection
* Input validation with Pydantic
* Parameterized queries via SQLAlchemy
* HTTPS required in production

---

# 🚀 Local Development

```bash
pip install -r requirements.txt
alembic upgrade head
uvicorn app.main:app --reload
```

API Docs available at:

```
http://localhost:8000/docs
```

---



---

# 🧪 Testing

Recommended:

* pytest
* pytest-asyncio
* httpx

Run:

```
pytest
```

---

# 🏥 Designed for Thai Healthcare

* INR safety range 2.0 – 3.0
* Warfarin-specific safety logic
* Thai-language optimized
* Emergency integration: 1669

---

# 💚 Responsible AI in Healthcare

Coaguard Backend is built around one principle:

AI assists.
Doctors decide.
Rules enforce.
Patients stay safe.

---

## 🛡️ Coaguard

Safer Warfarin Management Through Structured Data and Intelligent Design.
