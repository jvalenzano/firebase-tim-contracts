# Firebase Studio Refactor Plan for TIM Contracts Application

## 🔧 Project Scope
Refactor the **Contract Preparation** module of the Timber Information Manager (TIM) using **Firebase Studio** and Firebase tools. Support all 6 gates of the timber sale lifecycle:

> **Gate 1: Plan → Gate 2: Design → Gate 3: Prepare → Gate 4: Advertise → Gate 5: Bid → Gate 6: Award**

### Objectives
- Support modular gate-based workflows
- Enforce role-based access
- Automate critical processes (locking, reporting)
- Integrate document handling, audit logs, and cloud storage
- Deploy in a secure, scalable Firebase-native environment

---

## 🧱 Firebase Products & Features
| Firebase Product       | Purpose                                                        |
|------------------------|----------------------------------------------------------------|
| Firebase Studio        | Low-code UI builder for workflow-centric forms and pages       |
| Firestore              | NoSQL database for contracts, metadata, gate data              |
| Firebase Auth          | Role-based authentication (custom claims)                      |
| Firebase Functions     | Business logic: locking gates, generating reports              |
| Firebase Extensions    | PDF generation, integrations with GSheet, etc.                 |
| Cloud Storage          | Uploads: documents, appraisals, awards                         |
| Firebase Hosting       | Frontend deployment for USDA users                             |
| App Check              | Security for frontend-to-backend communications                |
| Analytics + Logging    | Track usage, gate transitions, and role-specific activity logs |

---

## 🔄 Gate Mapping to Firebase Features
| TIM Gate        | Key Functions                               | Firebase Components                                           |
|-----------------|----------------------------------------------|---------------------------------------------------------------|
| **Gate 1: Plan**    | Volume estimates, certification reports     | Firestore, Auth, Studio Form                                  |
| **Gate 2: Design**  | NEPA decision data, revised volumes         | Firestore, Auth, Functions                                    |
| **Gate 3: Prepare** | Cruise methods, NATCRS data, funding        | Firestore, Cloud Storage, Studio Form, Functions              |
| **Gate 4: Advertise**| Appraisal plans, bid documents, provisions | Firestore, Studio Forms, PDF Extensions                       |
| **Gate 5: Bid**     | Bid entry, report generation                | Firestore, Studio UI, Functions                               |
| **Gate 6: Award**   | Award letters, billing, contracts           | Firestore, PDF Gen, Cloud Storage, Functions                  |

---

## 🔐 Role-Based Access (RBAC)
| TIM Role               | Firebase Custom Claim Mapping |
|------------------------|-------------------------------|
| TIM_CONTRACT_PREPARATION | role: `contract_preparer`    |
| TIM_CONTRACT_AGENT     | role: `contract_agent`         |
| TIM_FINANCE            | role: `finance_officer`        |
| TIM_DATA_MANAGER       | role: `data_admin`             |

> 🔐 Role logic should be enforced via Firestore rules, Studio conditionals, and Function-level guards.

---

## 🗂️ Firestore Data Structure (Conceptual)
```plaintext
/timber_sales/{saleId}
  ├── metadata
  ├── gateStatus: { gate1, gate2, ..., gate6 }
  ├── gate1_plan: {...}
  ├── gate2_design: {...}
  ├── gate3_prepare: {...}
  ├── gate4_advertise: {...}
  ├── gate5_bid: {...}
  ├── gate6_award: {...}
  └── history: [ {action, gate, by, timestamp} ]
```

---

## 🔁 Suggested Studio Bindings
| Studio Form       | Bound Collection / Fields                     |
|-------------------|-----------------------------------------------|
| Gate 1 Plan        | `timber_sales/{id}/gate1_plan`               |
| Gate 2 Design      | `timber_sales/{id}/gate2_design`             |
| Gate 3 Prepare     | `timber_sales/{id}/gate3_prepare`            |
| Gate 4 Advertise   | `timber_sales/{id}/gate4_advertise`          |
| Gate 5 Bid         | `timber_sales/{id}/gate5_bid`                |
| Gate 6 Award       | `timber_sales/{id}/gate6_award`              |
| Project Nav        | `timber_sales` (filtered by `metadata.status`) |
| Role Check         | `request.auth.token.role`                    |

---

## 🔐 Firestore Security Rules (Example)
```js
match /timber_sales/{saleId} {
  allow read: if request.auth != null;

  match /gate1_plan {
    allow update: if request.auth.token.role in ['contract_preparer', 'data_admin'];
  }
  match /gate2_design {
    allow update: if request.auth.token.role in ['contract_preparer', 'data_admin'];
  }
  match /gate3_prepare {
    allow update: if request.auth.token.role == 'contract_preparer';
  }
  match /gate6_award {
    allow update: if request.auth.token.role == 'contract_agent';
  }
  // etc.
}
```

---

## 🎨 UI Mockup Notes (by Gate)
Use Studio forms and components to support:
- File uploads for appraisals, bid docs, and awards
- Table inputs for cruise data, bidders
- Tabbed UI for plans (KV, SSF, Brush)
- Role-gated field editing
- Gate lock status indicators

📌 _[Insert mockup image here]_ 📌

---

## 🚀 Deployment Workflow
1. Design schema and pages in Firebase Studio
2. Bind each form to Firestore collections
3. Set Firestore rules with role/claim logic
4. Use Functions to lock gates, generate PDFs
5. Deploy via Firebase Hosting (internal USDA use)
6. Share with stakeholders using Preview Channels

---

## ✅ Benefits of Firebase Studio Approach
- Low-code, rapid development
- Fully serverless and scalable
- Integrates authentication, storage, workflows
- Centralized role enforcement and audit logging
- GCP-native and extensible with APIs or BigQuery

---

## 📦 JSON Firestore Schema
📁 _See: `firebase_schema_gate1_to_gate6.json` for complete object structure._

---

## 📌 Placeholder Diagrams
- Architecture Diagram → _[insert TIM Firebase architecture image here]_ 
- Gate-to-Firestore Mapping → _[insert Mermaid or UML]_ 
- Studio UI Flow → _[insert screen wireframes or page flow chart]_

---

## ✨ Next Steps
- [ ] Build Studio Pages for all gates
- [ ] Apply Firestore Rules
- [ ] Create sample records in `timber_sales`
- [ ] Validate role-based editing + locking
- [ ] Connect PDF extension and deploy preview
