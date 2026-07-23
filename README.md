# n8n_IITproject project by: Aditi Neve
# 🎓 Smart University Management Platform

An end-to-end academic administration automation platform built entirely on **n8n**, covering the full student lifecycle — from registration to graduation — using AI-powered decision-making, scheduled monitoring, and automated communication.

> **Domain:** Education &nbsp;|&nbsp; **Automation Engine:** [n8n](https://n8n.io) &nbsp;|&nbsp; **AI Provider:** Google Gemini
> **Prepared by:** Aditi Neve

---

## 📌 Overview

Universities manage thousands of students across departments, and much of the administrative overhead — registration, attendance tracking, assignment grading, result publication, and certificate issuance — is still handled manually. This project automates that entire pipeline using five interconnected n8n workflows, backed by a shared Google Sheets database and a centralized audit/logging system.

## 🧩 Workflows in This Repository

| # | Workflow | Trigger | Nodes | Purpose |
|---|----------|---------|-------|---------|
| 1 | **Student Registration & Enrollment** | Google Sheets Trigger (Form response) | 5 | Generates Student ID, records enrollment, sends welcome email, logs the event |
| 2 | **Attendance Tracking & Low-Attendance Alert** | Schedule (Cron — every Friday 5 PM) | 5 | Calculates attendance %, alerts student + counselor if below 75% |
| 3 | **AI Assignment Submission & Evaluation** | Webhook | 8 | Downloads submission, grades it via Gemini AI, routes low grades to human review, publishes results |
| 4 | **Grade Aggregation & Centralized Audit Logger** | Execute Workflow Trigger (called by other workflows) | 6 | Logs every system event, filters by severity, escalates critical errors to admins |
| 5 | **End-of-Term Certificate Generation & Reports** | Webhook (manual, admin-triggered) | 6 | Fetches eligible graduates, generates certificate PDFs, stores them in Drive, emails students |

## 🏗️ Architecture

```
                    ┌─────────────────────────────┐
                    │   University Master Database │
                    │  (Students | Attendance |     │
                    │   Grades tabs - Google Sheets) │
                    └───────────────┬─────────────────┘
                                    │  read / write
        ┌───────────┬───────────────┼───────────────┬───────────────┐
        │            │               │               │               │
  ┌─────▼────┐ ┌─────▼─────┐  ┌─────▼──────┐ ┌──────▼───────┐ ┌─────▼─────┐
  │Workflow 1│ │ Workflow 2 │  │ Workflow 3  │ │  Workflow 4  │ │ Workflow 5│
  │Student   │ │Attendance  │  │AI Assignment│ │Grade Aggreg. │ │Certificate│
  │Registr.  │ │ Tracking   │  │Submission & │ │& Centralized │ │Generation │
  │& Enroll. │ │& Alerts    │  │Evaluation   │ │ Audit Logger │ │& Reports  │
  └────┬─────┘ └─────┬──────┘  └──────┬──────┘ └──────▲───────┘ └─────┬─────┘
       │             │                │                │              │
       └─────────────┴──── calls into Workflow 4 ───────┘              │
                                                                        │
                             all workflows email students via Gmail ◄──┘
```

## 🛠️ Tech Stack & Integrations

- **Automation Engine:** n8n (self-hosted)
- **Database:** Google Sheets (Students, Attendance, Grades, Audit Log tabs)
- **File Storage:** Google Drive
- **Email:** Gmail (OAuth2)
- **AI Grading:** Google Gemini API (`gemini-2.5-flash`)
- **PDF Generation:** PDFShift API (HTML → PDF conversion)
- **Forms:** Google Forms (linked to Sheets for registration intake)

## 📂 Repository Structure

```
smart-university-management-platform/
├── README.md
├── docs/
│   └── Smart_University_Platform_Workflow_Documentation.pdf
├── workflows/
│   ├── Workflow_1_Student_Registration.json
│   ├── Workflow_2_Attendance_Tracking.json
│   ├── Workflow_3_AI_Assignment_Evaluation.json
│   ├── Workflow_4_Audit_Logger.json
│   └── Workflow_5_Certificate_Generation.json
└── screenshots/
    ├── workflow1_canvas.png
    ├── workflow2_canvas.png
    ├── workflow3_canvas.png
    ├── workflow4_canvas.png
    └── workflow5_canvas.png
```

## 🚀 Setup Instructions

### Prerequisites
- A self-hosted n8n instance (or n8n Cloud)
- Google Cloud OAuth credentials for Sheets, Drive, and Gmail
- A [Google Gemini API key](https://aistudio.google.com/apikey)
- A [PDFShift API key](https://pdfshift.io) (free tier)

### Import a Workflow
1. Open your n8n instance
2. Go to **Workflows → "+" → Import from File**
3. Select the desired `.json` file from the `workflows/` folder
4. Configure credentials for each node (Google Sheets, Google Drive, Gmail, Gemini/PDFShift)
5. Replace placeholder values (`PASTE_YOUR_..._ID_HERE`) with your actual Google Sheet IDs, Drive folder IDs, etc.

### Required Google Sheet Structure
Create one spreadsheet ("University Master Database") with these tabs:
- **Students**: Student ID, Name, Email, Program, Enrollment Year, Status, Registered At, Graduation_Year, GPA, Eligible
- **Attendance**: Student_ID, Name, Email, Counselor_Email, Total_Classes, Attended_Classes
- **Grades**: Student_ID, Assignment_ID, Grade, Feedback, Status
- **Audit Log**: Timestamp, Action, Status, Severity, Details

## ✨ Key Features Demonstrated

- ✅ AI-powered decision making (Gemini auto-grading)
- ✅ Human-in-the-loop approval gate (low-confidence grades routed for manual review)
- ✅ Centralized error handling, logging & audit trail
- ✅ Scheduled workflows (Cron)
- ✅ Webhook-triggered workflows
- ✅ Conditional branching (IF / Switch nodes)
- ✅ Batch looping (Split In Batches for certificate generation)

## 📄 Documentation

Full project documentation — covering business context, stakeholders, architecture, and a detailed breakdown of every workflow and node — is available in [`docs/Smart_University_Platform_Workflow_Documentation.pdf`](./docs/Smart_University_Platform_Workflow_Documentation.pdf).

## 📸 Screenshots

Live, successfully-executed canvases for all five workflows are available in the [`screenshots/`](./screenshots) folder.

---

*This project was built as part of an academic assignment demonstrating multi-workflow automation, AI integration, and human-in-the-loop system design using n8n.*
