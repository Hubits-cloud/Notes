# 🧭 IT Service Management (ITSM) — Complete Notes

Framework focus: **ITIL 4** (current ITSM body of knowledge), plus adjacent frameworks.

---

# 📘 Module 1: ITSM Fundamentals

## 1.1 What is ITSM?

- **ITSM (IT Service Management)**: The set of policies, processes, and practices for designing, delivering, managing, and improving IT services to meet business needs.
- Focus is on **services and value**, not just technology.
- **Framework ≠ ITSM**: ITIL, COBIT, ISO 20000 are *frameworks/standards* used to *do* ITSM.
- **Goal**: Deliver IT as a service that produces measurable value for the customer.

## 1.2 Core Definitions

- **Service**: A means of enabling value co-creation by facilitating outcomes customers want, **without** the customer managing specific costs and risks.
- **Value**: The perceived benefits, usefulness, and importance of something. Subjective — defined by the consumer.
- **Outcome**: A result for a stakeholder enabled by one or more outputs.
- **Output**: A tangible/intangible deliverable of an activity.
- **Cost**: Money spent on an activity/resource (removed cost = benefit; imposed cost = risk).
- **Risk**: A possible event that could cause harm or loss, or make objectives harder to achieve.

## 1.3 Utility vs Warranty

- **Utility ("Fit for purpose")**: *What* the service does — functionality that meets a need. Removes constraints / improves performance.
- **Warranty ("Fit for use")**: *How* the service performs — availability, capacity, security, continuity.
- **Value requires BOTH**: A service is only valuable if it works (utility) *and* works reliably (warranty).

## 1.4 Products vs Services

- **Product**: A configuration of an organization's **resources** designed to offer value (often not seen directly by the consumer).
- **Service**: Built from one or more products; what the consumer actually receives.
- **Service Offering**: A description of services aimed at consumers — made of **goods**, **access to resources**, and **service actions**.

## 1.5 Stakeholders in Service Relationships

- **Service Provider**: The organization delivering the service.
- **Service Consumer**: The organization/person receiving it. Split into 3 roles:
  1. **Customer** — defines requirements & is accountable for outcomes.
  2. **User** — actually uses the service.
  3. **Sponsor** — authorizes budget/spend.
- **Other stakeholders**: Employees, suppliers, investors, regulators, society.

## 1.6 Service Relationship Concepts

- **Service Relationship**: Cooperation between provider and consumer.
- **Service Provision**: Provider activities (managing resources, access, service levels).
- **Service Consumption**: Consumer activities (using resources, requesting, receiving).
- **Service Relationship Management**: Joint activities to co-create value.

## ✅ Summary

- ITSM = managing IT **as a service** to co-create value.
- Value = **Utility (fit for purpose)** + **Warranty (fit for use)**.
- Consumer roles: Customer (requirements), User (uses), Sponsor (pays).
- Value is **co-created**, not just delivered.

---

# 📗 Module 2: ITIL 4 Overview

## 2.1 What is ITIL?

- **ITIL**: The most widely adopted ITSM framework — a set of best practices, not a rigid standard.
- Owned by **PeopleCert / AXELOS**.
- **Descriptive & adaptable** ("adopt and adapt"), not prescriptive.

## 2.2 History

| Version | Focus |
| ------- | ----- |
| ITIL v1 (1989) | Original UK government (CCTA) library. |
| ITIL v2 (2000) | Process-focused, grouped into disciplines. |
| ITIL v3 (2007/2011) | **Service Lifecycle** (5 stages). |
| **ITIL 4 (2019)** | **Value co-creation**, SVS, integration with Agile/DevOps/Lean. |

## 2.3 ITIL 4 — Two Key Components

1. **The Service Value System (SVS)** — how all components work together to create value.
2. **The Four Dimensions Model** — four perspectives that must be balanced for effective service management.

## 2.4 Key Shift in ITIL 4

- v3 = **Processes** in a linear lifecycle.
- ITIL 4 = **Practices** feeding a flexible **Service Value Chain**.
- Emphasis on **holistic**, **flexible**, **value-driven** work aligned with modern DevOps/Agile ways of working.

## ✅ Summary

- ITIL = adaptable best-practice ITSM framework ("adopt and adapt").
- ITIL 4 centers on **value co-creation** via the **SVS** and **Four Dimensions**.
- Moved from *processes/lifecycle* (v3) to *practices/value chain* (v4).

---

# 🧩 Module 3: The Four Dimensions of Service Management

> All four must be considered for **balanced, effective** value creation. Neglecting one causes degraded service.

## 3.1 Organizations & People

- Structure, culture, roles, responsibilities, competencies, communication.
- Right skills + right leadership + healthy culture.

## 3.2 Information & Technology

- Information, knowledge, and the technologies needed for the service.
- Includes: data management, security requirements, workflows, AI, cloud, comms tech.
- Ask: Is it compatible? Compliant? Secure? The right tech for the job?

## 3.3 Partners & Suppliers

- Relationships with other organizations involved in design, deploy, deliver, support.
- **Service integration models**: insource, outsource, co-source.
- Governed by contracts, SLAs, and shared goals.

## 3.4 Value Streams & Processes

- **Value Stream**: The series of steps an org takes to create and deliver value.
- **Process**: A set of interrelated activities turning inputs into outputs.
- Defines *how* the org's parts work together.

## 3.5 External Factors — PESTLE

- Factors outside the org's control that influence all four dimensions:
  * **P**olitical, **E**conomic, **S**ocial, **T**echnological, **L**egal, **E**nvironmental.

## ✅ Summary

- Four dimensions: **Organizations & People**, **Information & Technology**, **Partners & Suppliers**, **Value Streams & Processes**.
- Ignoring any dimension → blind spots and poor service.
- **PESTLE** = external constraints surrounding all four.

---

# ⚙️ Module 4: The Service Value System (SVS)

## 4.1 Purpose

- **SVS**: Describes how all components and activities of the organization work **together** to enable value creation.
- **Input**: Opportunity & Demand.
- **Output**: Value.

## 4.2 SVS Components

1. **Guiding Principles** — universal recommendations that guide behavior.
2. **Governance** — how the org is directed & controlled.
3. **Service Value Chain (SVC)** — the operating model / core activities.
4. **Practices** — 34 sets of organizational resources for doing work.
5. **Continual Improvement** — ongoing enhancement across all areas.

## 4.3 Inputs

- **Opportunity**: Options to add value / improve.
- **Demand**: Need for products and services from internal/external consumers.

## 4.4 The Point of the SVS

- Reduce **silos** and prevent **"work in isolation"**.
- Provide a **flexible, adaptable** structure that responds to demand and delivers value continuously.

## ✅ Summary

- SVS turns **opportunity + demand → value**.
- 5 components: **Guiding Principles, Governance, Service Value Chain, Practices, Continual Improvement**.
- Designed to break silos and stay adaptable.

---

# 🧭 Module 5: The Seven Guiding Principles

> Universal, enduring recommendations that apply in almost any situation. Not sequential — use all as relevant.

1. **Focus on Value**
   - Everything links back to value for stakeholders. Know who the consumer is and what they value.

2. **Start Where You Are**
   - Don't rip-and-replace blindly. Assess the current state; reuse what works. Measure directly, don't assume.

3. **Progress Iteratively with Feedback**
   - Break work into small, manageable chunks. Use feedback loops to adjust each iteration.

4. **Collaborate and Promote Visibility**
   - Work across boundaries; the right people involved. Poor visibility → poor decisions. Reduce hidden work.

5. **Think and Work Holistically**
   - No component works alone. Consider the whole system (all four dimensions).

6. **Keep It Simple and Practical**
   - Use the minimum steps to achieve the objective. Eliminate wasteful activity. Outcome-based thinking.

7. **Optimize and Automate**
   - Optimize *first* (make it as effective as possible), *then* automate. Don't automate a broken process.

## ✅ Summary

- 7 principles guide all ITSM decisions and behavior.
- Memory aid: **Value → Start → Iterate → Collaborate → Holistic → Simple → Optimize/Automate**.
- **Optimize before you automate**.

---

# 🔗 Module 6: The Service Value Chain (SVC)

## 6.1 Overview

- **SVC**: The central operating model of the SVS — the set of interconnected activities to create/deliver value.
- **Flexible**: Activities combine in different sequences (**value streams**) depending on the scenario.
- Each activity uses **practices** and converts **inputs → outputs**.

## 6.2 The Six Value Chain Activities

1. **Plan**
   - Shared understanding of vision, status, and improvement direction across all four dimensions.

2. **Improve**
   - Continual improvement of products, services, and practices across the value chain.

3. **Engage**
   - Good understanding of stakeholder needs; ongoing engagement with all stakeholders (users, customers, suppliers, partners).

4. **Design & Transition**
   - Ensure products/services continually meet expectations for quality, cost, and time-to-market.

5. **Obtain / Build**
   - Ensure components (services, hardware, software) are available when and where needed, to spec.

6. **Deliver & Support**
   - Ensure services are delivered and supported to agreed specs and stakeholder expectations.

## 6.3 Value Streams

- A **value stream** = a specific combination of value chain activities + practices designed for a scenario (e.g., resolving an incident, onboarding a new user).
- Same 6 activities, **many possible paths**.

## ✅ Summary

- SVC = flexible operating model with **6 activities**: Plan, Improve, Engage, Design & Transition, Obtain/Build, Deliver & Support.
- **Engage** and **Deliver & Support** interface most with stakeholders.
- Activities are chained into **value streams** for each scenario.

---

# 🔄 Module 7: Continual Improvement

## 7.1 Concept

- **Continual Improvement**: Recurring org activity to align services with changing needs — happens at *every* level.
- Both an **SVS component**, a **value chain activity**, and a **practice**.

## 7.2 The Continual Improvement Model (7 Steps)

1. **What is the vision?** — business vision & objectives.
2. **Where are we now?** — assess current state (baseline).
3. **Where do we want to be?** — define target state / measurable goals.
4. **How do we get there?** — plan the improvements.
5. **Take action** — execute the plan.
6. **Did we get there?** — check/measure results.
7. **How do we keep the momentum going?** — embed & repeat.

## 7.3 Continual Improvement Register (CIR)

- **CIR**: A structured database/document to capture, track, and prioritize improvement ideas.
- Prevents good ideas from being lost.

## 7.4 Key Metrics for Improvement

- **CSF (Critical Success Factor)**: A condition that must be met for success.
- **KPI (Key Performance Indicator)**: A measurable value showing how well a CSF/objective is met.
- **Baseline**: A measured starting point used for comparison.

## ✅ Summary

- Continual improvement runs at all levels, all the time.
- 7-step model: **Vision → Current state → Target → Plan → Act → Measure → Sustain**.
- Track ideas in the **CIR**; measure with **CSFs** and **KPIs**.

---

# 🗂️ Module 8: ITIL Practices — Overview

- **Practice**: A set of organizational resources designed for performing work or accomplishing an objective.
- ITIL 4 defines **34 practices** across **3 categories**:

| Category | Count | Origin |
| -------- | ----- | ------ |
| **General Management** | 14 | Business/general management domains. |
| **Service Management** | 17 | Core ITSM (developed in service industry). |
| **Technical Management** | 3 | Technology management domains. |

- Each practice draws on all **Four Dimensions** and can be used in multiple value chain activities.
- **Focus for foundation/practice**: Incident, Problem, Change, Service Request, Service Desk, SLM.

---

# 🧰 Module 9: General Management Practices (14)

> Key ones summarized (full list below).

## 9.1 Full List

- Architecture management, **Continual improvement**, **Information security management**, Knowledge management, Measurement & reporting, Organizational change management, Portfolio management, Project management, **Relationship management**, Risk management, Service financial management, Strategy management, **Supplier management**, Workforce & talent management.

## 9.2 Information Security Management

- Protects information: **Confidentiality, Integrity, Availability (CIA)** + Authentication & Non-repudiation.
- Balances **prevention, detection, correction**.
- Sets policies, controls, and risk-based security posture.

## 9.3 Relationship Management

- Establishes and nurtures links between the org and stakeholders (strategic & tactical).
- Identifies, analyzes, monitors, and improves relationships.

## 9.4 Supplier Management

- Manages suppliers and their performance to ensure seamless quality.
- Contracts, SLAs, evaluation, vendor relationships.
- Sourcing: **Insource / Outsource / Co-source (partnership)**.

## 9.5 Risk Management

- Identify, assess, and control risks to acceptable levels.
- Ongoing, not one-off.

## ✅ Summary

- 14 general management practices from broader business domains.
- Security = **CIA** + prevention/detection/correction.
- Supplier & Relationship management govern external and stakeholder links.

---

# 🛠️ Module 10: Service Management Practices (17)

> The operational heart of ITSM. Key practices detailed below.

## 10.1 Full List

- Availability mgmt, Business analysis, Capacity & performance mgmt, **Change enablement**, **Incident management**, IT asset management, Monitoring & event management, **Problem management**, Release management, **Service catalogue management**, **Service configuration management**, Service continuity mgmt, **Service design**, **Service desk**, **Service level management**, **Service request management**, Service validation & testing.

---

## 10.2 Service Desk

- **Purpose**: Single point of contact (**SPOC**) between the provider and users.
- Captures demand for incident resolution and service requests.
- **Key value**: User experience, empathy, communication — not just technical fixes.
- **Channels**: Phone, email, portal, chat, walk-up, chatbot.
- **Types**: Local, centralized, virtual, follow-the-sun, 24/7.
- **Escalation**:
  * **Functional (horizontal)** → to a team with more expertise (Tier 1 → 2 → 3).
  * **Hierarchical (vertical)** → to higher authority/management.

---

## 10.3 Incident Management

- **Incident**: An **unplanned interruption** to a service or **reduction in quality** of a service.
- **Purpose**: Restore normal service operation **as quickly as possible** to minimize business impact.
- **Priority = Impact × Urgency**.
  * **Impact**: How much of the business is affected (scale).
  * **Urgency**: How quickly resolution is needed.
- **Workarounds** are acceptable to restore service fast; permanent fix may follow via Problem Management.
- **Major Incident**: High impact/urgency — separate procedure, dedicated team, high visibility.
- **Swarming**: Multiple stakeholders work together initially until it's clear who is best placed.
- Best practice: log everything, use tooling, match to known errors, keep users informed.

### Priority Matrix (example)

| | High Urgency | Med Urgency | Low Urgency |
| --- | --- | --- | --- |
| **High Impact** | 1 (Critical) | 2 (High) | 3 (Medium) |
| **Med Impact** | 2 (High) | 3 (Medium) | 4 (Low) |
| **Low Impact** | 3 (Medium) | 4 (Low) | 5 (Planning) |

---

## 10.4 Problem Management

- **Problem**: A **cause, or potential cause, of one or more incidents**.
- **Known Error**: A problem that has been **analyzed but not resolved** (root cause understood).
- **Workaround**: A temporary way to reduce/eliminate impact without a full fix.
- **Purpose**: Reduce likelihood & impact of incidents by finding root causes and managing known errors.
- **Three phases**:
  1. **Problem Identification** — detect & log problems.
  2. **Problem Control** — analyze, prioritize, document known errors/workarounds.
  3. **Error Control** — manage known errors, drive permanent fixes (often via Change Enablement).
- **Incident vs Problem**:
  * Incident = *restore service now*.
  * Problem = *stop it happening again (root cause)*.

---

## 10.5 Change Enablement

- **Change**: The addition, modification, or removal of anything that could have a direct/indirect effect on services.
- **Purpose**: Maximize successful changes by ensuring proper **risk assessment**, **authorization**, and **scheduling**.
- **Three change types**:
  1. **Standard** — pre-authorized, low-risk, well-understood (e.g., password reset). Follows a defined procedure.
  2. **Normal** — assessed, scheduled, authorized by change authority (may need CAB).
  3. **Emergency** — must be done ASAP (e.g., security patch). Expedited assessment; often an **ECAB**.
- **Change Authority**: Person/group that authorizes a change.
- **CAB (Change Advisory Board)**: Advises on assessment, prioritization, scheduling of normal changes.
- **Change Schedule**: Plan of upcoming changes (helps avoid conflicts & plan resources).
- Balances **risk of change** vs **need to move fast** (Agile/DevOps friendly).

---

## 10.6 Service Request Management

- **Service Request**: A request from a user for something **normal/planned** (not a failure) — e.g., new laptop, access, info, password reset.
- **Purpose**: Handle all service requests efficiently and user-friendly.
- **Key traits**: Standardized, workflow-driven, often self-service/automated.
- **Distinct from incidents** — requests are business-as-usual, incidents are failures.

---

## 10.7 Service Level Management (SLM)

- **Purpose**: Set clear, business-based **targets** for service performance and ensure delivery is measured & managed against them.
- **SLA (Service Level Agreement)**: Documented agreement between provider and **customer** defining targets & responsibilities.
  * Good SLAs: simple, clearly worded, outcome/business-focused, agreed metrics.
  * Beware the **"watermelon SLA"** — green on the outside (metrics met), red inside (customer unhappy).
- Related agreements:
  * **OLA (Operational Level Agreement)**: Internal agreement between provider teams.
  * **UC (Underpinning Contract)**: External contract with a third-party supplier.
- SLM gathers info via: metrics, customer feedback, business reviews.

---

## 10.8 Service Configuration Management

- **Purpose**: Ensure accurate, reliable information about the **configuration** of services and the CIs that support them is available.
- **CI (Configuration Item)**: Any component that needs to be managed to deliver a service.
- **CMDB (Configuration Management Database)**: Stores CIs and their relationships.
- **CMS (Configuration Management System)**: Set of tools/data managing configuration data (may hold multiple CMDBs).
- Supports impact analysis for incidents, problems, and changes.

---

## 10.9 IT Asset Management (ITAM)

- **Purpose**: Plan and manage the full **lifecycle** of IT assets to maximize value, control costs, manage risk, support decisions.
- **IT Asset**: Any financially valuable component that can contribute to service delivery.
- Covers procurement, deployment, maintenance, retirement/disposal.
- **ITAM ≠ Config Mgmt**: ITAM = cost/lifecycle/ownership; Config Mgmt = relationships/how things fit together.

---

## 10.10 Monitoring & Event Management

- **Event**: Any change of state significant for the management of a service/CI.
- **Purpose**: Systematically observe services & CIs, record & report state changes (events).
- **Event types**:
  * **Informational** — no action needed (log only).
  * **Warning** — approaching a threshold; may need attention.
  * **Exception** — breach/error; requires action (may trigger an incident).

---

## 10.11 Release & Deployment (quick view)

- **Release Management**: Make new/changed services and features **available for use**.
- **Deployment Management** (technical): Move new/changed components into live environments.
- A release can be deployed without being enabled; deployment ≠ release ≠ change.

## ✅ Summary

- **Service Desk** = SPOC; owns user experience.
- **Incident** = restore fast; **Problem** = fix root cause; **Known Error** = understood, unresolved.
- **Change** types: Standard / Normal / Emergency.
- **Request** = planned/normal user ask.
- **SLM**: SLA (customer), OLA (internal), UC (supplier).
- **CMDB/CMS** store **CIs**; **ITAM** manages asset lifecycle & cost.

---

# 💻 Module 11: Technical Management Practices (3)

## 11.1 Deployment Management

- Moves new/changed hardware, software, docs, and processes to live (or test) environments.
- Approaches: **phased**, **continuous delivery**, **big bang**, **pull**.

## 11.2 Infrastructure & Platform Management

- Oversees infrastructure and platforms used by the org.
- Enables monitoring of tech solutions; includes cloud & IaC.

## 11.3 Software Development & Management

- Ensures applications meet stakeholder needs (functionality, reliability, maintainability, usability).
- Aligns with Agile/DevOps and continuous integration/delivery.

## ✅ Summary

- 3 technical practices: **Deployment**, **Infrastructure & Platform**, **Software Development & Management**.
- Strongly linked to DevOps, CI/CD, and cloud.

---

# 📏 Module 12: Key Concepts, Agreements & Metrics

## 12.1 Agreement Types

| Term | Between | Nature |
| ---- | ------- | ------ |
| **SLA** | Provider ↔ Customer | Service targets & responsibilities. |
| **OLA** | Internal provider teams | Internal support commitments. |
| **UC** | Provider ↔ External supplier | Contractual, legally binding. |

## 12.2 Metrics & Success

- **CSF (Critical Success Factor)**: Must-be-true condition for success.
- **KPI (Key Performance Indicator)**: Measures progress toward a CSF/objective.
- **Metric**: A measurement being tracked.
- **Baseline**: Reference point for comparison.

## 12.3 Common ITSM Metrics

- **MTTR (Mean Time To Restore/Resolve)** — average time to restore service after an incident.
- **MTBF (Mean Time Between Failures)** — reliability measure.
- **FCR (First Contact Resolution)** — % resolved on first contact.
- **SLA compliance %**, **backlog**, **reopen rate**, **CSAT** (customer satisfaction).

## 12.4 Priority = Impact × Urgency

- **Impact**: Extent of business effect (how many/how critical).
- **Urgency**: Time sensitivity of resolution.
- Combined into a **priority matrix** to sequence work.

## ✅ Summary

- **SLA** (customer), **OLA** (internal), **UC** (supplier).
- **CSF** = condition for success; **KPI** = measures it.
- Track **MTTR, MTBF, FCR, CSAT, SLA %**.
- Prioritize incidents by **Impact × Urgency**.

---

# 🌐 Module 13: Related Frameworks & Standards

## 13.1 COBIT

- **COBIT (Control Objectives for Information and Related Technologies)** — by ISACA.
- **Governance** framework: aligns IT with business goals, focuses on control, audit, and risk.
- Complements ITIL (COBIT = govern/control; ITIL = manage/operate).

## 13.2 ISO/IEC 20000

- The **international standard** for ITSM (certifiable).
- Orgs can be *audited & certified* against it; ITIL helps *achieve* it.

## 13.3 DevOps

- Culture + practices uniting **Dev** and **Ops** for faster, reliable delivery.
- Emphasizes **CI/CD, automation, feedback loops, shared responsibility**.
- ITIL 4 explicitly integrates DevOps ways of working.

## 13.4 Agile

- Iterative, incremental delivery with fast feedback (Scrum, Kanban).
- Aligns with guiding principle "progress iteratively with feedback".

## 13.5 Lean

- Maximize value, **eliminate waste**, optimize flow.
- Aligns with "keep it simple and practical".

## 13.6 Six Sigma

- Data-driven method to reduce defects and variation (**DMAIC**: Define, Measure, Analyze, Improve, Control).

## ✅ Summary

- **COBIT** = governance/control; **ISO/IEC 20000** = certifiable ITSM standard.
- **ITIL** integrates **DevOps, Agile, Lean** for modern delivery.
- **Six Sigma / Lean** = quality & waste-reduction methods.

---

# 🎯 Master Cheat Sheet

- **Value** = Utility (fit for purpose) + Warranty (fit for use), **co-created**.
- **Consumer roles**: Customer (requirements) · User (uses) · Sponsor (pays).
- **Four Dimensions**: Orgs & People · Info & Tech · Partners & Suppliers · Value Streams & Processes (+ **PESTLE**).
- **SVS** = Guiding Principles · Governance · Service Value Chain · Practices · Continual Improvement.
- **7 Principles**: Value → Start where you are → Iterate w/ feedback → Collaborate/visibility → Holistic → Simple → Optimize & automate.
- **SVC (6)**: Plan · Improve · Engage · Design & Transition · Obtain/Build · Deliver & Support.
- **CI model (7)**: Vision → Current → Target → Plan → Act → Measure → Sustain.
- **Practices**: 34 total = 14 General + 17 Service + 3 Technical.
- **Incident** = restore fast; **Problem** = root cause; **Known Error** = understood-unresolved; **Workaround** = temp fix.
- **Change** = Standard · Normal · Emergency (CAB/ECAB).
- **Priority** = Impact × Urgency.
- **Agreements**: SLA (customer) · OLA (internal) · UC (supplier).
