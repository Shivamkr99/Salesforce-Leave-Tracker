# 🏖️ Salesforce Leave Tracker

A **Salesforce-based Leave Management application** built with **Apex, Lightning Web Components (LWC), and Aura** to manage and track employee leave requests.

The project demonstrates how Salesforce **server-side Apex controllers**, **Lightning components**, **custom objects**, and **metadata** work together to create a deployable Salesforce application.

---

## ✨ Features

* 📝 **Leave Request Management** — Manage employee leave requests within Salesforce.
* 👤 **User-Based Leave Tracking** — Retrieve leave requests associated with the current Salesforce user.
* 👨‍💼 **Manager Leave Management** — Retrieve leave requests for users managed by the current user.
* 📊 **Leave Status Tracking** — Supports statuses such as `Pending`, `Approved`, and `Rejected`.
* 💬 **Manager Comments** — Store and display comments associated with leave requests.
* ⚙️ **Apex Server-Side Logic** — Apex controllers handle Salesforce data retrieval and application logic.
* 🧩 **Lightning UI** — Frontend components are implemented using LWC and/or Aura.
* 🗃️ **Custom Salesforce Objects** — Leave request data is persisted using Salesforce custom object metadata.
* 🚀 **Salesforce CLI Deployment** — Project metadata can be deployed to a Salesforce org using SFDX/Salesforce CLI.
* 🧪 **Sample Data Utility** — Includes an Apex utility for creating sample leave request records.

---

## 🛠️ Tech Stack

| Technology                         | Usage                             |
| ---------------------------------- | --------------------------------- |
| **Salesforce Platform**            | Application platform              |
| **Apex**                           | Server-side logic and controllers |
| **Lightning Web Components (LWC)** | User interface                    |
| **Aura Components**                | Lightning component framework     |
| **JavaScript**                     | Lightning component development   |
| **XML**                            | Salesforce metadata configuration |
| **SOQL**                           | Salesforce data querying          |
| **Salesforce CLI (SFDX)**          | Deployment and project management |

---

## 📂 Project Structure

```text
force-app/
└── main/
    └── default/
        ├── classes/
        │   ├── LeaveRequestSampleData.cls
        │   ├── LeaveRequstController.cls
        │   ├── LeaveRequestSampleData.cls-meta.xml
        │   └── LeaveRequstController.cls-meta.xml
        │
        ├── aura/
        │   └── .eslintrc.json
        │
        ├── lwc/
        │   └── <Lightning Web Components>
        │
        └── objects/
            └── <Custom Object and Field Metadata>
```

---

## 🏗️ Architecture

The application follows a simple Salesforce application architecture:

```text
                 ┌─────────────────────────┐
                 │     Salesforce User     │
                 └────────────┬────────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │   LWC / Aura Components │
                 │       User Interface    │
                 └────────────┬────────────┘
                              │
                              │ Apex Calls
                              ▼
                 ┌─────────────────────────┐
                 │    Apex Controllers    │
                 │  Business / Server Logic│
                 └────────────┬────────────┘
                              │
                              │ SOQL
                              ▼
                 ┌─────────────────────────┐
                 │ LeaveRequest__c Object  │
                 │   Salesforce Database   │
                 └─────────────────────────┘
```

### How the components work together

**1. Lightning Components**

The frontend is implemented using Lightning components located inside the `lwc/` and `aura/` directories.

These components provide the user interface for interacting with leave requests.

**2. Apex Controllers**

Apex classes provide server-side functionality and retrieve leave request records from Salesforce.

The main controller includes methods for retrieving:

* The current user's leave requests
* Leave requests belonging to users managed by the current user

**3. Custom Object**

The `LeaveRequest__c` custom object stores leave request information such as:

* User
* From Date
* To Date
* Reason
* Status
* Manager Comment

**4. Sample Data**

`LeaveRequestSampleData.cls` provides sample leave records for development and testing.

---

## ⚙️ Apex Components

### `LeaveRequstController.cls`

The controller provides server-side methods for retrieving leave requests.

#### `getMyLeaves()`

Retrieves leave requests belonging to the currently logged-in Salesforce user.

```apex
WHERE User__c = :UserInfo.getUserId()
```

Records are returned in descending order based on creation date.

#### `getLeaveRequests()`

Retrieves leave requests for users whose manager is the currently logged-in user.

```apex
WHERE User__r.ManagerId = :UserInfo.getUserId()
```

This allows the application to provide different visibility for employees and managers.

---

### `LeaveRequestSampleData.cls`

Provides sample leave request records for testing and development.

The utility creates records with different statuses:

```text
Approved
Pending
Rejected
```

This makes it easier to populate a Salesforce org with sample data while developing the application.

---

## 🗃️ Leave Request Data Model

The application uses a custom Salesforce object:

```text
LeaveRequest__c
```

Important fields include:

| Field                | Purpose                                           |
| -------------------- | ------------------------------------------------- |
| `User__c`            | Salesforce user associated with the leave request |
| `From_Date__c`       | Leave start date                                  |
| `To_Date__c`         | Leave end date                                    |
| `Reason__c`          | Reason for requesting leave                       |
| `Status__c`          | Current leave status                              |
| `Manager_Comment__c` | Comment from the manager                          |

---

## 🚀 Deployment

This project is structured according to the Salesforce DX project format.

### Prerequisites

Before deploying the project, make sure you have:

* A Salesforce Developer Org or Sandbox
* Salesforce CLI
* A Salesforce user with appropriate permissions
* VS Code with Salesforce extensions (recommended)

### Clone the repository

```bash
git clone <YOUR-GITHUB-REPOSITORY-URL>
cd <YOUR-REPOSITORY-NAME>
```

### Authenticate with Salesforce

```bash
sf org login web
```

Follow the browser instructions to authenticate your Salesforce org.

### Deploy the project

```bash
sf project deploy start --source-dir force-app/main/default
```

After successful deployment, the Apex classes, Lightning components, and Salesforce metadata will be available in the target org.

---

## 🧪 Sample Data

After deployment, the sample data utility can be executed from Salesforce Developer Console or another Apex execution environment.

Example:

```apex
LeaveRequestSampleData.createData();
```

This creates sample leave requests for the currently logged-in user.

---

## 🔐 Security

The Apex classes use:

```apex
public with sharing class
```

This ensures that the Apex code respects the sharing rules of the current Salesforce user.

The controller also uses:

```apex
UserInfo.getUserId()
```

to identify the currently logged-in user when retrieving leave records.

---

## 📌 Current Scope

The current implementation focuses on:

* Leave request data management
* Apex server-side controllers
* Employee leave retrieval
* Manager-based leave retrieval
* Lightning UI
* Custom Salesforce metadata
* Sample data generation
* Salesforce CLI deployment

---

## 🔮 Future Improvements

Potential improvements for future versions include:

* ✨ Leave request submission directly from the LWC UI
* 🔔 Automated notifications for managers and employees
* 📅 Leave calendar visualization
* 📊 Leave analytics and dashboards
* ✅ Manager approval/rejection actions
* 🔄 Automated approval workflows using Salesforce Flow
* 🔐 Additional permission and access controls
* 🧪 Apex unit test classes with higher test coverage
* 📱 Improved responsive Lightning UI

---

## 🎯 Learning Objectives

This project demonstrates practical experience with:

* Salesforce Platform development
* Apex programming
* Apex controllers
* SOQL queries
* Lightning Web Components
* Aura Components
* Salesforce custom objects
* Salesforce metadata
* User-context-based data retrieval
* Salesforce DX project structure
* Salesforce CLI deployment

---

## 👨‍💻 Author

**Shivam Kumar**

B.Tech — Information Technology
Kalinga Institute of Industrial Technology (KIIT)

---

## ⭐ Project Highlights

> **Salesforce Platform + Apex + LWC/Aura + SOQL + Salesforce CLI**

A practical Salesforce project demonstrating the integration of **Lightning frontend components with Apex server-side logic and Salesforce custom data models**.

If you find this project useful, consider giving the repository a ⭐.
