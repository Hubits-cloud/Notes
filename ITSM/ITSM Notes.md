# 🧭 IT Service Management (ITSM) — Complete Notes

Framework focus: **ITIL 4** (current ITSM body of knowledge), with the ITIL v3 lifecycle, the practical/modern practice view (Atlassian JSM), related frameworks, and a full acronym glossary at the end.

- **ITSM** = *IT Service Management*
- **ITIL** = *IT Infrastructure Library* (formerly *Information Technology Infrastructure Library*)

---

# 📘 Module 1: ITSM Fundamentals

## 1.1 What is ITSM?

- **ITSM (IT Service Management)**: The set of policies, processes, and practices for designing, delivering, managing, and improving IT services to meet business needs.
- Focus is on **services and value**, not just technology.
- **Framework ≠ ITSM**: ITIL, COBIT, and ISO/IEC 20000 are *frameworks/standards* used to *do* ITSM.
- **Goal**: Deliver IT as a service that produces measurable value for the customer.

## 1.2 Why learn ITSM & ITIL as a technician?

- **Efficiency**: Structured methods to organize and optimize IT services → less wasted time/cost.
- **Quality**: Focus on reliable services aligned to business goals.
- **Fault handling**: Guidelines for errors/problems → less downtime, better business continuity.
- **Scalability**: Standard processes let the IT estate grow without losing control.
- **Customer orientation**: Understand and adapt to user needs; strengthens IT ↔ business relationship.
- **Best practice**: Years of accumulated experience → avoid reinventing mistakes.
- **Certifications**: ITIL certs are widely requested by employers → better career prospects.

## 1.3 Core Definitions

- **Service**: A means of enabling value co-creation by facilitating outcomes customers want, **without** the customer managing specific costs and risks.
- **Value**: The perceived benefits, usefulness, and importance of something. Subjective — defined by the consumer.
- **Outcome**: A result for a stakeholder enabled by one or more outputs.
- **Output**: A tangible/intangible deliverable of an activity.
- **Cost**: Money spent on an activity/resource. Value can be created by *removing* cost (a benefit) and threatened by *imposed* cost (a risk).
- **Risk**: A possible event that could cause harm/loss or make objectives harder to achieve.

## 1.4 Utility vs Warranty

- **Utility ("Fit for purpose")**: *What* the service does — functionality that meets a need. Removes constraints / improves performance.
- **Warranty ("Fit for use")**: *How* the service performs — availability, capacity, security, continuity.
- **Value requires BOTH**: A customer cannot benefit from something fit for purpose but not fit for use (or vice versa).

## 1.5 Products vs Services

- **Product**: A configuration of an organization's **resources** designed to offer value (often not seen directly by the consumer).
- **Resources**: The four dimensions — people, information & technology, value streams & processes, and partners & suppliers.
- **Service**: Built from one or more products; what the consumer actually receives.
- **Service Offering**: A description of services aimed at consumers — made of **goods**, **access to resources**, and **service actions**.

## 1.6 Stakeholders in Service Relationships

- **Service Provider**: The organization delivering the service (can be internal and/or external).
- **Service Consumer**: The organization/person receiving it. Split into 3 roles:
  1. **Customer** — defines requirements & is accountable for outcomes.
  2. **User** — actually uses the service.
  3. **Sponsor** — authorizes the budget/spend.
- An organization can be **both** provider and consumer at the same time.
- **Other stakeholders**: Employees, suppliers, investors, regulators, society.

## 1.7 Service Relationship Concepts

- **Service Relationship**: Cooperation between provider and consumer.
- **Service Provision**: Provider activities (managing resources, access, service levels).
- **Service Consumption**: Consumer activities (using resources, requesting, receiving).
- **Service Relationship Management**: Joint activities to co-create value.
- **Value is a balancing act**: A relationship is only valuable when the positive effects (supported outcomes, removed costs/risks) outweigh the negative ones (affected outcomes, introduced costs/risks).

## ✅ Summary

- ITSM = managing IT **as a service** to co-create value.
- Value = **Utility (fit for purpose)** + **Warranty (fit for use)**.
- Consumer roles: **C**ustomer (requirements), **U**ser (uses), **S**ponsor (pays).
- Value is **co-created**, and only exists when positives outweigh negatives.

---

# 📗 Module 2: ITIL Overview & History

## 2.1 What is ITIL?

- **ITIL (IT Infrastructure Library)**: The most widely adopted ITSM framework — a set of best practices, not a rigid standard.
- Vendor-neutral, non-prescriptive: **"adopt and adapt"** to the organization.
- Provides a **common language** and set of practices → easier collaboration across teams, orgs, and suppliers.
- Owned/managed by **AXELOS** (a joint venture of the UK Cabinet Office and Capita); **PeopleCert** is the licensed **Examination Institute (EI)**.
- ITIL inspired other frameworks/standards such as **COBIT** and **ISO/IEC 20000**.

## 2.2 History of ITIL

| Version | Year | Key point |
| ------- | ---- | --------- |
| **ITIL v1** | 1989 | Original UK government (**CCTA**) library of books covering IT operations (fault, change, config mgmt). Mostly used in UK & Netherlands. |
| **ITIL v2** | 2000–2004 | More structured; 2 core books — **Service Delivery** & **Service Support** — covering 10 processes + the service desk. |
| **ITIL v3** | 2007 (2011 update) | **Service Lifecycle** — 5 core books, 26 processes + 4 functions; focus on business value & customer satisfaction. |
| **ITIL 4** | 2019 | **Value co-creation**; **SVS** + **Four Dimensions**; integrates Lean, Agile, DevOps; more modular & flexible. |

## 2.3 The ITIL v3 Service Lifecycle (still widely referenced)

- **Service Strategy** — plan IT services & strategies that support business goals.
- **Service Design** — design/develop services & processes to meet business requirements.
- **Service Transition** — build, test, and deploy new/changed services into production.
- **Service Operation** — run and maintain services in production (day-to-day).
- **Continual Service Improvement (CSI)** — continuously improve services & processes from feedback.
- **4 functions** underpinned the processes: Service Desk, Technical Management, Application Management, IT Operations Management.

## 2.4 The "ITSM model wheel" (vendor/tooling view)

- Many ITSM tools depict ITSM as a wheel with **Service Strategy** at the centre, surrounded by: **Service Catalog, Service Desk, Incident Management, Problem Management, Change Management, Reports & Dashboards, Continual Service Improvement, Project Management, Asset Management, Policy & Procedure Management**.
- Useful as a practical map of what an ITSM platform (e.g. Jira Service Management, ServiceNow) actually implements.

## 2.5 ITIL 4 — Two Key Components

1. **The Service Value System (SVS)** — how all components work together to create value.
2. **The Four Dimensions Model** — four perspectives that must be balanced for effective service management.

## 2.6 Benefits of ITIL 4

- Enables businesses to compete in the modern/digital world.
- Improved quality and faster delivery of value to customers.
- Supports digital transformation; promotes a **holistic** view of products/services.
- Easy to combine with **Lean, Agile, DevOps**.
- Retains proven ITIL know-how while giving a roadmap to evolve.

## ✅ Summary

- ITIL = adaptable best-practice ITSM framework ("adopt and adapt"), owned by AXELOS, examined by PeopleCert.
- History: v1 (1989) → v2 (2000, Delivery/Support) → v3 (2007, 5-stage lifecycle) → **v4 (2019, SVS + value)**.
- ITIL 4 centers on **value co-creation** via the **SVS** and **Four Dimensions**.

---

# 🧩 Module 3: The Four Dimensions of Service Management

> All four must be considered for **balanced, holistic** value creation. Neglect one and services become undeliverable or inefficient.

## 3.1 Organizations & People

- Structure, culture, roles, responsibilities, competencies, communication.
- Right skills + right leadership + a culture that encourages collaboration.
- Can include customers, supplier staff, and other stakeholders.

## 3.2 Information & Technology

- The information/knowledge needed **plus** the technologies to manage and deliver the service.
- Includes applications, databases, communications, workflow systems, knowledge bases, and newer tech (**AI**, mobile, **cloud**).
- Considers **security** and **regulatory compliance**; for many services, information management is the *primary* means of enabling value.

## 3.3 Partners & Suppliers

- Relationships, contracts, and agreements with other orgs involved in design, delivery, and support.
- **Sourcing models**: insource, outsource, co-source (partnership).
- **SIAM (Service Integration And Management)** is one model for coordinating multiple suppliers.
- Sourcing decisions weigh: strategic focus, corporate culture, cost effectiveness, specialist knowledge, variable demand.

## 3.4 Value Streams & Processes

- **Value Stream**: A series of steps to create and deliver value to consumers (map it to find improvement opportunities).
- **Process**: A set of interrelated activities turning inputs into outputs; defines sequence, dependencies, and who's involved.
- Defines *how* the org's parts work together (the delivery model).

## 3.5 External Factors — PESTLE

- Factors outside the org's control that constrain/influence all four dimensions:
  * **P**olitical, **E**conomic, **S**ocial, **T**echnological, **L**egal, **E**nvironmental.

## ✅ Summary

- Four dimensions: **Organizations & People**, **Information & Technology**, **Partners & Suppliers**, **Value Streams & Processes**.
- Ignoring any dimension → blind spots and poor service.
- **PESTLE** = the external constraints surrounding all four.

---

# ⚙️ Module 4: The Service Value System (SVS)

## 4.1 Purpose

- **SVS**: Describes how all components and activities of the organization work **together** to enable value creation, integration, and coordination.
- **Input**: Opportunity & Demand → **Output**: Value.
- Each org's SVS interfaces with others, forming an **ecosystem** of value.

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

- Reduce **silos** and prevent **work in isolation**.
- Provide a **flexible, adaptable** structure that responds to demand and delivers value continuously.

## ✅ Summary

- SVS turns **opportunity + demand → value**.
- 5 components: **Guiding Principles, Governance, Service Value Chain, Practices, Continual Improvement**.
- Designed to break silos and stay adaptable.

---

# 🧭 Module 5: The Seven Guiding Principles

> Universal, enduring recommendations that apply in almost any situation, regardless of goals/strategy/structure. Not sequential — use all as relevant. (ITIL 4's principles are notably close to the **Agile Manifesto**.)

1. **Focus on Value**
   - Everything maps (directly or indirectly) to value for stakeholders. Know who's served and what they value. Includes **CX (customer experience)** and **UX (user experience)**, which are objective *and* subjective and must be actively managed.

2. **Start Where You Are**
   - Don't rip-and-replace blindly. Assess the current state; reuse what works. Measure directly — reports may not match reality.

3. **Progress Iteratively with Feedback**
   - Break work into small, manageable chunks (timeboxed, sequential or parallel). Feedback loops improve each iteration and surface risks/issues early.

4. **Collaborate and Promote Visibility**
   - Involve the right people in the right roles → better buy-in and decisions. Make work visible; hidden work breeds misunderstanding and poor decisions.

5. **Think and Work Holistically**
   - No service, practice, or team stands alone. Consider the whole system (all four dimensions) end-to-end.

6. **Keep It Simple and Practical**
   - Use the fewest steps needed. Outcome-based thinking. Eliminate anything that doesn't add value.

7. **Optimize and Automate**
   - **Optimize first** (make it effective), *then* automate. Automation handles repetitive tasks consistently → less human error, lower cost; humans focus on complex decisions.

## ✅ Summary

- 7 principles: **Value → Start where you are → Iterate w/ feedback → Collaborate/visibility → Holistic → Simple → Optimize/Automate**.
- **Optimize before you automate** (never automate a broken process).

---

# 🏛️ Module 6: Governance

- **Governance**: How the organization is **directed and controlled** by a **governing body** (board of directors or executives) accountable at the highest level.
- The governing body **evaluates, directs, and monitors** all activities, including service management, and is accountable for compliance with policies and external regulations.
- It should retain **oversight of the SVS** to ensure alignment with the org's objectives and priorities.
- Governance sits within the SVS (alongside the guiding principles) and can apply to the whole org or just part of it.

## ✅ Summary

- Governance = **evaluate, direct, monitor**, done by the governing body.
- Ensures the SVS stays aligned with organizational objectives and compliance.

---

# 🔗 Module 7: The Service Value Chain (SVC)

## 7.1 Overview

- **SVC**: The central **operating model** of the SVS — interconnected activities to create, deliver, and continually improve services.
- **Flexible**: Activities combine in different sequences (**value streams**) per scenario; adaptable to DevOps, centralized IT, multimodal, etc.
- Each activity uses **practices** and converts **inputs → outputs**.

## 7.2 The Six Value Chain Activities

1. **Plan** — shared understanding of vision, current status, and improvement direction across all four dimensions and all products/services.
2. **Improve** — continual improvement of products, services, and practices across all activities and dimensions.
3. **Engage** — understand stakeholder needs; ongoing engagement and good relationships with all stakeholders (users, customers, suppliers, partners). *Main stakeholder-facing activity.*
4. **Design & Transition** — ensure products/services continually meet expectations for **quality, cost, and time-to-market**.
5. **Obtain / Build** — ensure components (services, hardware, software) are available where/when needed, to spec.
6. **Deliver & Support** — ensure services are delivered and supported to agreed specs and expectations. *Main operational activity.*

## 7.3 Value Streams

- A **value stream** = a specific combination of value chain activities + practices designed for a scenario (e.g., resolving an incident, onboarding a user) — a "journey" from **demand → value**.
- Same 6 activities, **many possible paths**. **Value stream mapping** helps streamline and optimize.

## ✅ Summary

- SVC = flexible operating model with **6 activities**: Plan, Improve, Engage, Design & Transition, Obtain/Build, Deliver & Support.
- **Engage** and **Deliver & Support** interface most with stakeholders.
- Activities chain into **value streams** for each scenario.

---

# 🔄 Module 8: Continual Improvement

## 8.1 Concept

- **Continual Improvement**: Recurring activity to align services with changing needs — at **every** level, strategic to operational.
- It appears three ways in ITIL 4: an **SVS component**, a **value chain activity (Improve)**, and a **practice**.
- Works best when *everyone* has a continual-improvement mindset.

## 8.2 The Continual Improvement Model (7 Steps)

| Step | Question | Activity |
| ---- | -------- | -------- |
| 1 | **What is the vision?** | Link to business vision, mission, goals, objectives. |
| 2 | **Where are we now?** | Perform baseline assessments (know the starting point). |
| 3 | **Where do we want to be?** | Define measurable targets (know the destination). |
| 4 | **How do we get there?** | Define the improvement plan. |
| 5 | **Take action** | Execute (waterfall, Agile, or a mix). |
| 6 | **Did we get there?** | Evaluate metrics & KPIs; if not, iterate. |
| 7 | **How do we keep the momentum going?** | Market successes, embed new methods, build support for the next round. |

## 8.3 Continual Improvement Register (CIR)

- **CIR**: A structured database/document to capture, track, and prioritize improvement ideas so they aren't lost.

## 8.4 Key Metrics for Improvement

- **CSF (Critical Success Factor)**: A condition that must be met for success.
- **KPI (Key Performance Indicator)**: A measurable value showing how well a CSF/objective is met.
- **Baseline**: A measured starting point used for comparison.

## ✅ Summary

- Continual improvement runs at all levels, all the time.
- 7-step model: **Vision → Current state → Target → Plan → Act → Measure → Sustain**.
- Track ideas in the **CIR**; measure with **CSFs** and **KPIs**.

---

# 🗂️ Module 9: ITIL Practices — Overview

- **Practice**: A set of organizational resources designed for performing work or accomplishing an objective.
- ITIL 4 defines **34 practices** across **3 categories**:

| Category | Count | Origin |
| -------- | ----- | ------ |
| **General Management** | 14 | Adopted from general business management. |
| **Service Management** | 17 | Developed in the ITSM/service industry. |
| **Technical Management** | 3 | Adapted from technology management. |

- Every practice is subject to all **Four Dimensions** and supports multiple value chain activities.

## Practice guide structure (each ITIL 4 practice guide follows this)

- General information; **Purpose & description**; **Terms & concepts**; **Scope**; **Practice Success Factors (PSFs)**; **Key metrics**; value streams & processes; how it contributes to the SVC; processes & activities; **Organizations & people** (roles, competencies, structures); **Information & technology** (inputs/outputs, automation & tooling); **Partners & suppliers** (third-party relationships, sourcing).

---

# 🧰 Module 10: General Management Practices (14)

## 10.1 Full List

Architecture management, **Continual improvement**, **Information security management**, Knowledge management, Measurement & reporting, Organizational change management, Portfolio management, **Project management**, **Relationship management**, **Risk management**, Service financial management, Strategy management, **Supplier management**, Workforce & talent management.

## 10.2 Information Security Management

- Protects information: **CIA — Confidentiality, Integrity, Availability** (+ Authentication & Non-repudiation).
- Balances **prevention, detection, correction**; sets risk-based policies and controls.

## 10.3 Relationship Management

- Establishes and nurtures links between the org and its stakeholders (strategic & tactical); identifies, analyzes, monitors, and improves relationships.

## 10.4 Supplier Management

- Manages suppliers and their performance for seamless quality (contracts, SLAs, evaluation, vendor relationships).
- Sourcing: **Insource / Outsource / Co-source**; may use **SIAM** to integrate many suppliers.

## 10.5 Risk Management

- Identify, assess, and control risks to an acceptable level — ongoing, not one-off.

## 10.6 Project Management

- Balances keeping current operations running, transforming operations to compete, and continually improving products/services. Increasingly Lean/Agile/DevOps-influenced.

## ✅ Summary

- 14 general management practices drawn from broader business domains.
- Security = **CIA** + prevention/detection/correction.
- Supplier & Relationship management govern external and stakeholder links.

---

# 🛠️ Module 11: Service Management Practices (17)

> The operational heart of ITSM. Key practices detailed below, with the **modern/practical view** (as implemented in tools like Jira Service Management).

## 11.1 Full List

Availability mgmt, Business analysis, Capacity & performance mgmt, **Change enablement**, **Incident management**, **IT asset management**, **Monitoring & event management**, **Problem management**, **Release management**, **Service catalogue management**, **Service configuration management**, Service continuity mgmt, **Service design**, **Service desk**, **Service level management**, **Service request management**, Service validation & testing.

---

## 11.2 Service Desk

- **Purpose**: Single point of contact (**SPOC**) between the provider and users; captures demand for incidents and service requests.
- Modern focus: supporting **"people and business,"** not just fixing broken tech — arranging, explaining, coordinating.
- **Key value**: user experience, empathy, communication.
- **Channels**: phone, email, self-service portal, chat, walk-up, chatbot/virtual agent.
- **Types**: local, centralized, virtual, follow-the-sun, 24/7.
- **Escalation**:
  * **Functional (horizontal)** → to a team with more expertise (Tier 1 → 2 → 3).
  * **Hierarchical (vertical)** → to higher authority/management.

---

## 11.3 Incident Management

- **Incident**: An **unplanned interruption** to a service or **reduction in service quality**.
- **Purpose**: Restore normal service operation **as fast as possible** to minimize business impact.
- **Priority = Impact × Urgency** (Impact = scale of business effect; Urgency = how quickly resolution is needed).
- **Major Incident**: Significant business impact → dedicated, coordinated response.
- **Swarming**: Multiple stakeholders work together at the start until it's clear who's best placed.
- **Workarounds** are fine to restore service fast; the permanent fix may follow via Problem Management.

### Modern incident practices (high-velocity/DevOps)

- **On-call scheduling & alerting** (e.g. Opsgenie): rotations, escalation rules, multi-channel alerts; reduce **alert fatigue** by grouping/filtering noise.
- **ChatOps**: run response in chat (e.g. Slack) instead of phone bridges/war rooms; add chatbots/automation.
- **Runbooks**: documented, repeatable remediation steps attached to alerts (can cut resolution time significantly).
- **Monitoring integration**: system-detected incidents auto-raised from tools (Datadog, Nagios, etc.).
- **Status communication** (e.g. Statuspage): proactive mass updates build trust and deflect inbound tickets.
- **Post-Incident Review (PIR) / postmortem**: after resolution, run a **blameless** review to find root cause and preventive actions; link outputs back to the incident. (Most reviews get forgotten if not tracked — so track them.)
- **Metric focus**: lower **MTTR**; most response time is spent in investigation & diagnosis.

### Priority Matrix (example)

| | High Urgency | Med Urgency | Low Urgency |
| --- | --- | --- | --- |
| **High Impact** | 1 (Critical) | 2 (High) | 3 (Medium) |
| **Med Impact** | 2 (High) | 3 (Medium) | 4 (Low) |
| **Low Impact** | 3 (Medium) | 4 (Low) | 5 (Planning) |

---

## 11.4 Problem Management

- **Problem**: A **cause, or potential cause, of one or more incidents**.
- **Known Error**: A problem that's been **analyzed but not yet resolved** (root cause understood).
- **Workaround**: A temporary way to reduce/eliminate impact without a full fix.
- **Purpose**: Reduce the likelihood & impact of incidents by finding root causes and managing known errors.
- **Three phases**:
  1. **Problem Identification** — detect & log problems.
  2. **Problem Control** — analyze, prioritize, document known errors/workarounds.
  3. **Error Control** — manage known errors, drive permanent fixes (often via Change Enablement).
- **Prioritize by risk** — you don't need to analyze *every* problem; make real progress on the highest-risk ones.
- **Modern tip — blend incident + problem**: treat problem management as an extension of incident response (via the PIR) so it's a *single stream of work*, not a giant backlog. Prioritize PIRs for major/critical-service incidents.
- **Incident vs Problem**: Incident = *restore service now*; Problem = *stop it recurring (root cause)*.

---

## 11.5 Change Enablement

- **Change**: The addition, modification, or removal of anything that could directly/indirectly affect services.
- **Purpose**: Maximize successful changes by ensuring proper **risk assessment**, **authorization**, and **scheduling** — balancing beneficial change against protecting users from adverse effects.
- **Three change types**:
  1. **Standard** — pre-authorized, low-risk, routine, follows a documented procedure (e.g. add memory, new DB instance).
  2. **Normal** — assessed, scheduled, authorized by a change authority (may involve the CAB).
  3. **Emergency** — must happen ASAP (e.g. security patch, outage fix); expedited assessment, often an **ECAB**.
- **Change Authority**: Person/group that authorizes a change.
- **CAB (Change Advisory Board)**: Advises on assessment, prioritization, scheduling of normal changes.
- **Change Schedule**: Plan of upcoming changes (avoid conflicts, plan resources).

### Modern change practices (adaptive change enablement)

- Stop treating change as **one-size-fits-all**; classify by **risk** and use data from past changes to decide.
- **Make standard change the new normal**: analyze past changes, move low-risk ones to a **pre-approved/automated** standard path (real orgs have moved ~70% of changes this way).
- **Evolve the CAB from gatekeeper → enabler**: shift approval closer to the people doing the work; use **peer review**, daily standups, and automation for team-level changes (peer review is a top predictor of high performance).
- **Automated risk model**: score risk from form answers (business hours? easily rolled back? tested?) and auto-route standard/normal/emergency workflows.
- **DevOps change**: integrate with **CI/CD** so a code deploy auto-creates a change request, auto-assesses risk, and flags only high-risk ones for review; link changes to incidents for context.

---

## 11.6 Service Request Management

- **Service Request**: A request from a user for something **normal/planned** (not a failure) — new laptop, access, information, password reset.
- **Request fulfilment** manages the whole lifecycle of all service requests.
- **Purpose**: Handle requests efficiently and in a user-friendly, standardized, workflow-driven way (often self-service/automated).
- **Modern practice — "shift left"**: push fulfilment as close to the user/front line as possible.
  * **Self-service portal** + **searchable knowledge base** deflect tickets.
  * **Knowledge-centric**: surface the right KB articles/request types as users search (improves the more it's used).
  * Automate common requests (password resets, access, software provisioning); improves satisfaction and cuts cost.
- **Distinct from incidents** — requests are business-as-usual; incidents are failures.

---

## 11.7 Service Level Management (SLM)

- **Purpose**: Set clear, business-based **targets** for service performance and manage delivery against them.
- **SLA (Service Level Agreement)**: Documented agreement between provider and **customer** defining targets & responsibilities.
  * Good SLAs: simple, clearly worded, business/outcome-focused, agreed metrics.
  * Beware the **"watermelon SLA"** — green outside (metrics met) but red inside (customer unhappy).
- Related agreements:
  * **OLA (Operational Level Agreement)**: internal agreement between provider teams.
  * **UC (Underpinning Contract)**: external, legally binding contract with a third-party supplier.
- SLM inputs: metrics, customer feedback, business reviews.

---

## 11.8 Service Configuration Management

- **Purpose**: Ensure accurate, reliable information about the **configuration** of services and the CIs that support them is available where/when needed.
- **CI (Configuration Item)**: Any component that must be managed to deliver a service.
- **CMDB (Configuration Management Database)**: Stores CIs and their relationships.
- **CMS (Configuration Management System)**: The tools/data managing configuration information (may hold multiple CMDBs).
- **Service map / service model**: The high-level view of a service and its CIs/dependencies.
- Supports **impact analysis** for incidents, problems, and changes.
- **Balance effort vs value**: don't over-collect; detailed data on every component is costly and can deliver little value.

### Modern CMDB reality

- ~**80% of CMDB projects fail** — usually from **too wide a scope** up front.
- Fix: start **narrow** (1–2 critical services), grow the service map as you learn.
- **Data federation**: pull live data from source systems (AWS, Azure, device tools like Jamf/SCCM) instead of duplicating everything.
- Use **discovery** (agentless scanning) + **automation** to find CIs/relationships and keep data current; audit for accuracy.

---

## 11.9 IT Asset Management (ITAM)

- **Purpose**: Plan and manage the full **lifecycle** of IT assets to maximize value, control cost, and manage risk.
- **IT Asset**: Any financially valuable component that can contribute to service delivery.
- Covers procurement → deployment → maintenance → retirement/disposal.
- **ITAM ≠ Config Mgmt**: ITAM = cost/ownership/lifecycle; Config Mgmt = relationships/how things fit together. (Modern tools often store assets and CIs together.)

---

## 11.10 Monitoring & Event Management

- **Event**: Any change of state significant for managing a service/CI.
- **Purpose**: Systematically observe services & CIs and record/report events.
- **Event types**:
  * **Informational** — no action needed (log only).
  * **Warning** — approaching a threshold; may need attention.
  * **Exception** — a breach/error; requires action (may trigger an incident).

---

## 11.11 Release & Deployment

- **Release Management**: Make new/changed services and features **available for use**.
- **Deployment Management** (technical practice): Move new/changed components into live (or test/staging) environments.
- **Deployment approaches**: phased, continuous delivery, big bang, pull.
- Deployment ≠ release ≠ change (a component can be deployed but not yet released/enabled).

## ✅ Summary

- **Service Desk** = SPOC; owns the user experience.
- **Incident** = restore fast; **Problem** = fix root cause; **Known Error** = understood, unresolved.
- **Change** types: Standard / Normal / Emergency; modern CAB = enabler, not gatekeeper.
- **Request** = planned/normal user ask; **shift left** with self-service + knowledge.
- **SLM**: SLA (customer), OLA (internal), UC (supplier).
- **CMDB/CMS** store **CIs**; start narrow (80% of CMDBs fail from over-scoping). **ITAM** manages asset lifecycle & cost.

---

# 💻 Module 12: Technical Management Practices (3)

## 12.1 Deployment Management

- Moves new/changed hardware, software, docs, and processes into live/test/staging environments.
- Approaches: phased, continuous delivery, big bang, pull.

## 12.2 Infrastructure & Platform Management

- Oversees the infrastructure and platforms used by the org (incl. cloud & Infrastructure-as-Code); enables monitoring of tech solutions.

## 12.3 Software Development & Management

- Ensures applications meet stakeholder needs (functionality, reliability, maintainability, usability); aligns with Agile/DevOps and CI/CD.

## ✅ Summary

- 3 technical practices: **Deployment**, **Infrastructure & Platform**, **Software Development & Management**.
- Strongly linked to DevOps, CI/CD, and cloud.

---

# 🧱 Module 13: Practical ITSM — The Three Service Groupings

> How the practices cluster in real tooling/operations (Atlassian JSM view). Handy for day-to-day work.

## 13.1 Service Delivery

- Getting the right services **built and changed**: IT business management (demand/intake), **change enablement**, **service configuration management (CMDB)**, knowledge management.
- Themes: capture business demand via self-service, agile project delivery, adaptive change, service-focused CMDB.

## 13.2 Service Operations

- Keeping services **running**: **incident management** and **problem management**.
- Themes: proactive incident playbook, on-call/alerting, ChatOps, runbooks, status comms, blameless PIRs, blend incident + problem.

## 13.3 Service Support

- Helping **users**: **request management**, **service desk**, knowledge management, reporting.
- Themes: shift-left self-service, knowledge-centric support, automation, mobile, KPIs/CSAT.

## 13.4 Enterprise Service Management (ESM)

- **ESM**: Extending ITSM processes/tools **beyond IT** to other teams (HR, Facilities, Legal, Finance, Procurement) — same practices, org-wide.
- Removes silos; common use cases include employee onboarding, contract review, supply ordering.

## 13.5 Adoption best practices (getting started)

- Embrace a **team-centric**, collaborative, transparent culture.
- **Start where you are** — inventory existing services/tools/people before building.
- **Top-down**: begin with the most critical business services (check the last few months of tickets).
- **Quick wins via MVP (Minimal Viable Product)** — iterate rather than big-bang.
- **Match tooling to maturity** — buy only what you need; scale later.
- **Scale & celebrate** — communicate wins to drive adoption.

## ✅ Summary

- Practices cluster into **Delivery** (build/change), **Operations** (run), **Support** (users).
- **ESM** applies the same ITSM approach across the whole business.
- Adopt iteratively: start small, deliver quick wins, scale.

---

# 📏 Module 14: Key Concepts, Agreements & Metrics

## 14.1 Agreement Types

| Term | Between | Nature |
| ---- | ------- | ------ |
| **SLA (Service Level Agreement)** | Provider ↔ Customer | Service targets & responsibilities. |
| **OLA (Operational Level Agreement)** | Internal provider teams | Internal support commitments. |
| **UC (Underpinning Contract)** | Provider ↔ External supplier | Contractual, legally binding. |

## 14.2 Metrics & Success

- **CSF (Critical Success Factor)**: Must-be-true condition for success.
- **KPI (Key Performance Indicator)**: Measures progress toward a CSF/objective (chosen to reflect business goals *and* drive the right behaviors).
- **Metric**: A measurement being tracked. **Baseline**: reference point for comparison.
- ⚠️ Beware perverse incentives — e.g. pushing MTTR down can tempt agents to close tickets before the issue is truly solved.

## 14.3 Common ITSM / Support Metrics

- **Service support**: mean time to resolve, mean time to respond, request backlog size, created vs resolved, **SLA success rate**, cost per ticket, **CSAT**.
- **Incident**: incidents over time, **MTBF**, **MTTA** (mean time to acknowledge), **MTTR**, % resolved within SLA, % outages due to incidents, **uptime**.
- **Change**: change success/acceptance rate, average change lead time, **change failure rate**, number/duration of change-related incidents, audit/compliance findings.
- **CSAT (Customer Satisfaction)**: often a short survey (e.g. 5-star + comment) after resolution.

## 14.4 Priority = Impact × Urgency

- **Impact**: extent of business effect (how many / how critical).
- **Urgency**: time sensitivity of resolution.
- Combined into a **priority matrix** to sequence work.

## ✅ Summary

- **SLA** (customer), **OLA** (internal), **UC** (supplier).
- **CSF** = condition for success; **KPI** = measures it — pick metrics that drive the right behavior.
- Core metrics: **MTTR, MTTA, MTBF, FCR, CSAT, SLA %, change failure rate, uptime**.
- Prioritize incidents by **Impact × Urgency**.

---

# 🎓 Module 15: The ITIL 4 Certification Scheme

- Four levels, all built on **ITIL Foundation**.

| Level / Stream | Modules | Notes |
| -------------- | ------- | ----- |
| **ITIL Foundation** | 1 | Intro to ITIL 4; end-to-end operating model. Entry point / prerequisite for all higher modules. |
| **ITIL Managing Professional (MP)** | 4 | Practical/technical: *Create, Deliver & Support*; *Drive Stakeholder Value*; *High-velocity IT*; *Direct, Plan & Improve*. |
| **ITIL Strategic Leader (SL)** | 2 | *Direct, Plan & Improve* (**universal**, shared with MP) + *Digital & IT Strategy*. |
| **ITIL Master** | — | Prove real-world application of ITIL. Needs MP + SL (or old v3 Expert) **and** ≥5 years IT SM leadership experience. No formal training/exam. |

- **Direct, Plan & Improve** is the **universal module** bridging the MP and SL streams.
- Accredited training is **mandatory** for the advanced modules.

## ✅ Summary

- Path: **Foundation → MP and/or SL → Master**.
- **Direct, Plan & Improve** is shared across MP and SL.

---

# 🧑‍🔧 Module 16: The T-Shaped Professional

- Modern ITSM professionals are **"T-shaped."**
- **Vertical bar of the "T"** = deep expert knowledge in a core discipline (e.g. ITIL/ITSM).
- **Horizontal bar** = ability and willingness to collaborate **across** other disciplines (Agile, DevOps, Cloud, Leadership, Cyber security, ITSM tooling).
- ITIL 4's advanced certs deliberately broaden beyond deep ITIL process knowledge → wider perspective and cross-team collaboration.

---

# 🌐 Module 17: Related Frameworks & Standards

## 17.1 AXELOS Best-Practice Portfolio (siblings of ITIL)

- **PRINCE2 (PRojects IN Controlled Environments)**: project management method.
- **PRINCE2 Agile**: PRINCE2 combined with Agile delivery.
- **MSP (Managing Successful Programmes)**: programme management.
- **AgileSHIFT**: enterprise agility / change readiness.
- **RESILIA**: cyber resilience best practice.

## 17.2 Other Frameworks & Methods

- **COBIT (Control Objectives for Information and Related Technologies)** — ISACA: **governance & control** framework (aligns IT with business, audit, risk). Complements ITIL (COBIT governs; ITIL manages/operates).
- **Agile Manifesto**: 4 values + 12 principles for iterative, people-centric delivery.
- **Lean IT**: Lean manufacturing principles applied to IT — maximize value, eliminate waste.
- **DevOps**: unites **Dev** and **Ops** for fast, reliable delivery (CI/CD, automation, feedback, shared responsibility). Explicitly embraced by ITIL 4.
- **Six Sigma**: data-driven defect/variation reduction (**DMAIC** — Define, Measure, Analyze, Improve, Control).
- **Kanban**: visual workflow / flow management.
- **SIAM (Service Integration And Management)**: coordinating multiple service providers/suppliers.
- **Cynefin**: a sense-making/decision-making framework.
- **IT4IT (The Open Group)**: reference architecture for managing a digital enterprise via value streams.
- **TOGAF (The Open Group Architecture Framework)**: enterprise architecture framework.

## 17.3 Relevant ISO/IEC Standards

- **ISO/IEC 20000**: the international **ITSM standard** (certifiable) — see Module 18.
- **ISO/IEC 27001**: Information Security Management standard.
- **ISO/IEC 14001**: Environmental management.
- **ISO 9000**: Quality management.

## ✅ Summary

- **COBIT** = governance/control; **ISO/IEC 20000** = certifiable ITSM standard.
- ITIL sits in the **AXELOS** portfolio (PRINCE2, MSP, AgileSHIFT, RESILIA) and integrates **DevOps, Agile, Lean, Six Sigma, SIAM**.

---

# 📜 Module 18: ISO/IEC 20000 — The Service Management Standard

- The primary **internationally recognized standard** for ITSM; organizations can be **audited and certified** against it.
- Specifies requirements to establish, implement, maintain, and continually improve a **Service Management System (SMS)** across the service lifecycle (plan, design, transition, deliver, improve).
- **Agnostic/independent** of any single framework — a common route to meeting it is by **adopting ITIL best practices**.
- Multi-part standard (requirements spec, application guidance, concepts/vocabulary, and how it maps to ITIL).
- **ITIL vs ISO/IEC 20000**: ITIL = best-practice guidance you *adopt*; ISO/IEC 20000 = the standard you get *certified* against.

## ✅ Summary

- ISO/IEC 20000 = certifiable ITSM standard built around an **SMS**.
- ITIL is the most common way to *achieve* its requirements.

---

# 👥 Module 19: ITSM / ITIL Roles & Responsibilities

> ITIL 4 talks about **roles** (a set of responsibilities), not job titles — one person can hold several roles, and one role can be shared. Roles are assigned to people; **accountability can't be delegated, responsibility can**.

## 19.1 Accountability model — RACI

- **RACI**: A responsibility-assignment matrix mapping roles to activities.
  * **R — Responsible**: does the work (can be several people).
  * **A — Accountable**: owns the outcome, signs off — **exactly one** per activity.
  * **C — Consulted**: gives input (two-way).
  * **I — Informed**: kept up to date (one-way).

## 19.2 Two foundational role concepts

- **Service Owner**: Accountable for a **specific service** end-to-end, across its whole lifecycle, regardless of where the components/teams sit.
- **Process/Practice Owner**: Accountable for the **design, documentation, and fitness** of a practice (the "how it should work").
- **Process/Practice Manager**: Accountable for the **day-to-day operational running** of the practice (the "make it happen now"). Often the same person in small teams.

## 19.3 ITIL 4 competency codes (LACMT)

Each role in an ITIL 4 practice guide is tagged with one or more competency letters:

| Code | Competency | Description |
| ---- | ---------- | ----------- |
| **L** | Leader | Decision-making, delegation, oversight, direction. |
| **A** | Administrator | Assign/prioritize, administer, report. |
| **C** | Coordinator / Communicator | Coordinate activity, communicate, maintain relationships. |
| **M** | Methods & techniques expert | Design/define work, apply methods, evaluate. |
| **T** | Technical expert | Deep technical/specialist knowledge. |

## 19.4 Quick-reference — key roles by practice

| Role | Practice | Core responsibility |
| ---- | -------- | ------------------- |
| **Change Manager** | Change Enablement | Owns the change process; assesses, authorizes, schedules changes; runs/chairs the CAB. |
| **Change Authority** | Change Enablement | Person/group that authorizes a specific change (varies by change type/risk). |
| **Incident Manager** | Incident Management | Owns the incident process; drives fast restoration; manages escalations. |
| **Major Incident Manager** | Incident Management | Coordinates the response to major incidents (the "incident commander"). |
| **Problem Manager** | Problem Management | Owns root-cause analysis; manages known errors & workarounds. |
| **Service Desk Analyst/Agent** | Service Desk | First-line SPOC; logs, triages, resolves or escalates. |
| **Service Desk Manager** | Service Desk | Owns service desk performance, staffing, and user experience. |
| **Service Level Manager** | SLM | Negotiates, monitors, and reports on SLAs/OLAs/UCs. |
| **Configuration Manager** | Service Configuration Mgmt | Owns the CMDB/CMS; ensures accurate CI data & relationships. |
| **Release Manager** | Release Management | Plans, schedules, and controls releases into live use. |
| **Deployment Manager** | Deployment Management | Moves components into live/test environments. |
| **Knowledge Manager** | Knowledge Management | Owns the knowledge base; drives capture, quality, and reuse. |
| **Availability Manager** | Availability Management | Ensures services meet agreed availability targets. |
| **Capacity Manager** | Capacity & Performance Mgmt | Ensures capacity/performance meets current & future demand. |
| **IT Asset Manager** | IT Asset Management | Owns asset lifecycle, cost, and disposal. |
| **Service Continuity Manager** | Service Continuity Mgmt | Owns DR/BC plans so services survive major disruption. |
| **Supplier/Vendor Manager** | Supplier Management | Owns supplier relationships, contracts, and performance. |

## 19.5 Roles worth knowing in detail

- **Change Manager**
  * Accountable for the change enablement practice and its performance.
  * Assesses risk/impact, decides change type (standard/normal/emergency), authorizes or routes to the right change authority.
  * Chairs the **CAB** (and **ECAB** for emergencies); maintains the **change schedule**.
  * Modern shift: less "gatekeeper," more **enabler** — sets up peer-review and automated/pre-approved paths for low-risk changes.

- **Incident Manager vs Major Incident Manager**
  * *Incident Manager* owns the process, metrics (MTTR), and continual improvement of incident handling.
  * *Major Incident Manager / Incident Commander* takes command during a major incident: coordinates responders, runs comms, keeps a single source of truth, and hands off to the PIR afterwards.

- **Problem Manager**
  * Turns recurring/high-risk incidents into investigations; documents **known errors** and **workarounds**; drives permanent fixes via change enablement.
  * Prioritizes by risk — not every problem gets analyzed.

- **Configuration Manager**
  * Defines CMDB scope (start narrow!), data model, and audit process; keeps CI relationships accurate for impact analysis.

- **Service Owner vs Process Owner** (common exam trap)
  * *Service Owner* = accountable for **one service** across all practices.
  * *Process/Practice Owner* = accountable for **one practice** across all services.

## 19.6 Modern / DevOps-adjacent roles

- **Incident Commander**: leads major-incident response (owns decisions/comms, not the fix).
- **On-call Engineer**: responds to alerts on a rota; uses runbooks to remediate.
- **Product Owner** (Agile): prioritizes the backlog and defines value for a product/service.
- **SRE (Site Reliability Engineer)**: applies software engineering to ops — reliability, automation, error budgets.

## ✅ Summary

- Roles ≠ job titles; **accountability is singular (one "A" in RACI)**, responsibility can be shared.
- **Service Owner** = one service, all practices; **Practice Owner** = one practice, all services.
- Each practice has a lead role: **Change Manager, Incident Manager, Problem Manager, Configuration Manager**, etc.
- ITIL 4 tags roles with **LACMT** competencies.
- Modern ops adds **Incident Commander, On-call Engineer, SRE, Product Owner**.

# 🎯 Master Cheat Sheet

- **ITSM** = managing IT as a service to co-create value; **ITIL** = the leading best-practice framework ("adopt and adapt").
- **Value** = Utility (fit for purpose) + Warranty (fit for use), **co-created**; only real when positives > negatives.
- **Consumer roles**: Customer (requirements) · User (uses) · Sponsor (pays).
- **History**: v1 (1989) → v2 (2000) → v3 (2007, 5-stage lifecycle) → **v4 (2019, SVS)**.
- **v3 lifecycle**: Strategy → Design → Transition → Operation → CSI.
- **Four Dimensions**: Orgs & People · Info & Tech · Partners & Suppliers · Value Streams & Processes (+ **PESTLE**).
- **SVS** = Guiding Principles · Governance · Service Value Chain · Practices · Continual Improvement.
- **7 Principles**: Value → Start where you are → Iterate w/ feedback → Collaborate/visibility → Holistic → Simple → Optimize & automate.
- **SVC (6)**: Plan · Improve · Engage · Design & Transition · Obtain/Build · Deliver & Support.
- **CI model (7)**: Vision → Current → Target → Plan → Act → Measure → Sustain.
- **Practices**: 34 = 14 General + 17 Service + 3 Technical.
- **Incident** = restore fast; **Problem** = root cause; **Known Error** = understood-unresolved; **Workaround** = temp fix.
- **Change** = Standard · Normal · Emergency (CAB/ECAB); modern CAB = enabler.
- **Priority** = Impact × Urgency.
- **Agreements**: SLA (customer) · OLA (internal) · UC (supplier).
- **Groupings**: Service Delivery (build/change) · Service Operations (run) · Service Support (users); **ESM** = beyond IT.
- **Certs**: Foundation → Managing Professional / Strategic Leader → Master.

---

# 🔤 Acronym Glossary

| Acronym | Full form | Meaning |
| ------- | --------- | ------- |
| **AI** | Artificial Intelligence | Machine-driven reasoning/automation used in modern ITSM tooling. |
| **CAB** | Change Advisory Board | Group advising on assessment/prioritization/scheduling of normal changes. |
| **CCTA** | Central Computer & Telecommunications Agency | UK gov body that created ITIL v1. |
| **CI** | Configuration Item | Any component managed to deliver a service. |
| **CI/CD** | Continuous Integration / Continuous Delivery (or Deployment) | Automated build-test-release pipeline (DevOps). |
| **CIA** | Confidentiality, Integrity, Availability | The core security triad. |
| **CIR** | Continual Improvement Register | Log of improvement ideas to track/prioritize. |
| **CMDB** | Configuration Management Database | Stores CIs and their relationships. |
| **CMS** | Configuration Management System | Tools/data managing configuration info (may hold multiple CMDBs). |
| **COBIT** | Control Objectives for Information and Related Technologies | IT governance & control framework (ISACA). |
| **CSAT** | Customer Satisfaction (score) | Metric of user satisfaction with support. |
| **CSF** | Critical Success Factor | A condition that must be met for success. |
| **CSI** | Continual Service Improvement | The improvement stage in ITIL v3. |
| **CX** | Customer Experience | The customer's overall experience of the service/provider. |
| **DMAIC** | Define, Measure, Analyze, Improve, Control | Six Sigma improvement cycle. |
| **ECAB** | Emergency Change Advisory Board | CAB variant for expedited emergency changes. |
| **EI** | Examination Institute | Body that runs ITIL exams (PeopleCert). |
| **ESM** | Enterprise Service Management | Applying ITSM practices beyond IT (HR, Facilities, Legal, etc.). |
| **FCR** | First Contact Resolution | % of issues resolved on first contact. |
| **HMAC** | Hash-based Message Authentication Code | Adds a key to hashing for origin authentication. |
| **IaC** | Infrastructure as Code | Managing infrastructure via machine-readable config. |
| **ISO/IEC** | International Org for Standardization / International Electrotechnical Commission | Standards bodies (e.g. 20000, 27001). |
| **IT4IT** | (The Open Group standard) | Reference architecture for managing a digital enterprise. |
| **ITAM** | IT Asset Management | Managing the lifecycle/cost of IT assets. |
| **ITIL** | IT Infrastructure Library | The leading ITSM best-practice framework. |
| **ITSM** | IT Service Management | Managing IT as a service to deliver value. |
| **KPI** | Key Performance Indicator | Measurable value showing progress toward a CSF/objective. |
| **LLQ** | Low Latency Queuing | (Networking QoS) priority queue for voice. |
| **MP** | (ITIL) Managing Professional | Advanced ITIL 4 certification stream. |
| **MTBF** | Mean Time Between Failures | Reliability metric. |
| **MTTA** | Mean Time To Acknowledge | Time to acknowledge an incident/alert. |
| **MTTR** | Mean Time To Restore/Resolve | Average time to restore service after an incident. |
| **MSP** | Managing Successful Programmes | AXELOS programme-management framework. |
| **MVP** | Minimal Viable Product | Smallest useful deliverable to iterate on. |
| **OLA** | Operational Level Agreement | Internal agreement between provider teams. |
| **PESTLE** | Political, Economic, Social, Technological, Legal, Environmental | External factors affecting the four dimensions. |
| **PIR** | Post-Incident Review | Blameless review after an incident (a.k.a. postmortem). |
| **PMO** | Project Management Office | Team governing projects/portfolio. |
| **PRINCE2** | PRojects IN Controlled Environments | AXELOS project-management method. |
| **PSF** | Practice Success Factor | Condition for a practice to fulfil its purpose. |
| **RESILIA** | (AXELOS) | Cyber resilience best-practice portfolio. |
| **SIAM** | Service Integration And Management | Model for integrating multiple suppliers. |
| **SL** | (ITIL) Strategic Leader | Advanced ITIL 4 certification stream. |
| **SLA** | Service Level Agreement | Provider ↔ customer service targets. |
| **SLM** | Service Level Management | Practice of setting/managing service-level targets. |
| **SMS** | Service Management System | The management system certified under ISO/IEC 20000. |
| **SPOC** | Single Point Of Contact | The service desk's role for users. |
| **SVC** | Service Value Chain | The 6-activity operating model at the core of the SVS. |
| **SVS** | Service Value System | How all ITIL components work together to create value. |
| **TOGAF** | The Open Group Architecture Framework | Enterprise architecture framework. |
| **UC** | Underpinning Contract | Provider ↔ external-supplier contract. |
| **UX** | User Experience | The user's experience of interacting with the service. |
