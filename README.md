# 🎫 Support Ticket Priority Prediction and Automated Assignment System Using Agentforce

![Salesforce](https://img.shields.io/badge/Salesforce-Platform-00A1E0?style=for-the-badge&logo=salesforce&logoColor=white)
![Agentforce](https://img.shields.io/badge/Agentforce-AI%20Agent-032D60?style=for-the-badge)
![Flow](https://img.shields.io/badge/Salesforce%20Flow-Automation-0D9DDA?style=for-the-badge)
![Status](https://img.shields.io/badge/Project-Completed-2EA44F?style=for-the-badge)

An intelligent Salesforce-based support ticket management system that analyzes customer support tickets, predicts their priority using keyword-based business logic, and automates assignment and escalation for high-priority cases using **Salesforce Flow and Agentforce**.

---

## 📌 Project Overview

This project demonstrates an automated support-ticket workflow built on the Salesforce platform.

The system:

- Receives customer support ticket information.
- Identifies the related customer Account.
- Retrieves the latest support ticket.
- Analyzes the ticket description.
- Classifies the ticket as **High, Medium, or Low** priority.
- Generates an action message for Agentforce.
- Supports automated task assignment/escalation for high-priority tickets.
- Exposes Flow outputs for use by an Agentforce AI agent.

The objective is to reduce manual ticket triage and provide a consistent backend workflow for support operations.

---

## 🎯 Business Objectives

| Objective | Implementation |
|---|---|
| ⚡ Faster ticket handling | Automatically identifies urgent tickets |
| 🤖 Reduced manual work | Uses Flow-based backend automation |
| 📊 Consistent prioritization | Applies predefined keyword rules |
| 👨‍💼 Intelligent assignment | High-priority tickets can be escalated to senior agents |
| 🧠 Agentforce integration | Flow inputs/outputs are mapped to an Agentforce action |

---

# 🏗️ System Architecture

```text
                 ┌───────────────────────┐
                 │      Customer /       │
                 │    Support Request    │
                 └───────────┬───────────┘
                             │
                             ▼
                 ┌───────────────────────┐
                 │ Support Ticket        │
                 │ Intelligence Object   │
                 └───────────┬───────────┘
                             │
                             ▼
                 ┌───────────────────────┐
                 │ Salesforce            │
                 │ Auto-Launched Flow    │
                 └───────────┬───────────┘
                             │
                    Get Account + Ticket
                             │
                             ▼
                 ┌───────────────────────┐
                 │ Keyword Analysis      │
                 │ Decision Logic        │
                 └───────────┬───────────┘
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
          HIGH            MEDIUM           LOW
              │              │              │
              ▼              ▼              ▼
       Escalate / Task     Normal          Normal
       Senior Agent       Handling        Handling
              │              │              │
              └──────────────┼──────────────┘
                             ▼
                 ┌───────────────────────┐
                 │ Agentforce AI Agent   │
                 │ Response / Action     │
                 └───────────────────────┘
```

---

# 🗃️ Custom Object Data Model

### Object

**Label:** Support Ticket Intelligence

**API Name:** `Support_Ticket_Intelligence__c`

| Field Label | API Name | Data Type | Purpose |
|---|---|---|---|
| Ticket Number | `Ticket_Number__c` | Auto Number | Generates ticket ID such as `TKT-0001` |
| Customer | `Customer__c` | Lookup (Account) | Links ticket to customer account |
| Contact | `Contact__c` | Lookup (Contact) | Links ticket to customer contact |
| Issue Type | `Issue_Type__c` | Picklist | Technical, Billing, General |
| Description | `Description__c` | Long Text Area | Stores support issue details |
| Priority Level | `Priority_Level__c` | Picklist | Low, Medium, High |
| Status | `Status__c` | Picklist | New, In Progress, Resolved |
| Created Date | `Created_Date__c` | Date | Ticket creation date |
| Assigned To | `Assigned_To__c` | Lookup (User) | Assigned support agent |
| SLA Breach Risk | `SLA_Breach_Risk__c` | Checkbox | Indicates SLA risk |

---

# ⚙️ Flow Automation

### Flow Type

**Auto-Launched Flow**

### Flow Name

`Support Ticket Intelligence`

### Input

```text
varAccountName
```

### Processing

#### 1. Get Account Records

The Flow searches for an Account using:

```text
varAccountName
```

#### 2. Get Latest Support Ticket

The Flow retrieves the latest `Support_Ticket_Intelligence__c` record related to the Account.

#### 3. Keyword Analysis

The ticket description is evaluated using predefined keywords.

### 🔴 High Priority

```text
urgent
not working
failure
```

### 🟡 Medium Priority

```text
issue
slow
delay
```

### 🟢 Low Priority

If none of the above keywords are detected, the ticket follows the Low Priority/default path.

---

# 🤖 Agentforce Configuration

### Topic

**Support Ticket Priority Analysis**

### API Name

```text
Support_Ticket_Priority_Analysis
```

### Agent Action

```text
Support_Ticket_Intelligence
```

### Flow

```text
Support_Ticket_Intelligence
```

### Input Parameter

```text
varAccountName
```

### Output Parameters

```text
varAccountId
varTicketId
varPriorityLevel
varAssignedTo
varActionMessage
```

Agentforce can use these outputs to provide a structured response and trigger the appropriate support workflow.

---

# 🔄 Priority Decision Logic

```text
                 Ticket Description
                         │
                         ▼
              ┌─────────────────────┐
              │ Contains "urgent"?  │
              │ "not working"?      │
              │ "failure"?          │
              └──────────┬──────────┘
                    YES  │  NO
                         │
                         ▼
                       HIGH
                         │
                         ▼
                  Senior Assignment


                    NO
                     │
                     ▼
              ┌─────────────────────┐
              │ Contains "issue"?   │
              │ "slow"?              │
              │ "delay"?             │
              └──────────┬──────────┘
                    YES  │  NO
                         │
                         ▼
                      MEDIUM
                         │
                         ▼
                    Normal Flow


                         NO
                         │
                         ▼
                        LOW
```

---

# 🧪 Example Test Cases

### Test Case 1 — High Priority

**Description**

```text
URGENT: Payment failure. Service is not working.
```

**Expected Result**

```text
Priority = High
```

The workflow can escalate the ticket and create/assign a high-priority Task to a senior support agent.

---

### Test Case 2 — Medium Priority

**Description**

```text
The application is slow and there is a delay while loading.
```

**Expected Result**

```text
Priority = Medium
```

---

### Test Case 3 — Low Priority

**Description**

```text
I need information about my account.
```

**Expected Result**

```text
Priority = Low
```

---

# 📸 Project Screenshots

## 1. Flow Debug Proof

![Flow Debug Proof](https://raw.githubusercontent.com/akshy485/support-ticket-intelligence/main/Flow_Debug_Proof.png)

The Flow Debug screen demonstrates the execution of the backend automation and its output values.

---

## 2. Support Ticket Intelligence Records

![Ticket Records Page](https://raw.githubusercontent.com/akshy485/support-ticket-intelligence/main/Ticket_Records_page.png)

The records page demonstrates the custom Support Ticket Intelligence object and its ticket data.

---

# 📁 Repository Structure

```text
support-ticket-intelligence/
│
├── README.md
├── sfdx-project.json
├── .gitignore
│
├── force-app/
│   └── main/
│       └── default/
│           ├── objects/
│           │   └── Support_Ticket_Intelligence__c/
│           │       ├── object-meta.xml
│           │       └── fields/
│           │           ├── Customer__c.field-meta.xml
│           │           ├── Contact__c.field-meta.xml
│           │           ├── Issue_Type__c.field-meta.xml
│           │           ├── Description__c.field-meta.xml
│           │           ├── Priority_Level__c.field-meta.xml
│           │           ├── Status__c.field-meta.xml
│           │           ├── Created_Date__c.field-meta.xml
│           │           ├── Assigned_To__c.field-meta.xml
│           │           └── SLA_Breach_Risk__c.field-meta.xml
│           │
│           ├── flows/
│           │   └── Support_Ticket_Intelligence.flow-meta.xml
│           │
│           └── permissionsets/
│               └── Support_Ticket_Intelligence_User.permissionset-meta.xml
│
└── docs/
    ├── Flow_Debug_Proof.png
    └── Ticket_Records_page.png
```

---

# 🚀 Deployment

Authenticate with a Salesforce org:

```bash
sf org login web --alias support-ticket-org
```

Deploy the Salesforce source:

```bash
sf project deploy start \
  --source-dir force-app/main/default \
  --target-org support-ticket-org
```

Open the org:

```bash
sf org open --target-org support-ticket-org
```

---

# 🛠️ Technologies Used

- **Salesforce Platform**
- **Salesforce Flow**
- **Agentforce**
- **Custom Objects & Fields**
- **Lookup Relationships**
- **Decision Elements**
- **Flow Variables**
- **Task Automation**
- **Salesforce DX / Metadata API**

---

# 📈 Project Status

| Component | Status |
|---|---|
| Custom Object | ✅ Completed |
| Custom Fields | ✅ Completed |
| Support Ticket Records | ✅ Tested |
| Auto-Launched Flow | ✅ Completed |
| Keyword Priority Logic | ✅ Tested |
| High-Priority Assignment Logic | ✅ Designed |
| Agentforce Topic | ✅ Configured |
| Agentforce Action | ✅ Mapped |
| Flow Debug Testing | ✅ Completed |
| GitHub Documentation | ✅ Ready |

---

# 👨‍💻 Project

**Support Ticket Priority Prediction and Automated Assignment System Using Agentforce**

Built as a Salesforce/Agentforce automation project demonstrating intelligent ticket triage, workflow automation, and AI-agent integration.
