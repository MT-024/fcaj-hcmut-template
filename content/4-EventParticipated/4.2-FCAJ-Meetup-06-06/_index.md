---
title: "FCAJ Meetup — 06/06/2026"
date: 2026-06-06
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
includeInReport: false
---

# Summary Report: FCAJ Community Meetup (06/06/2026)

### Event Information

| | |
| --- | --- |
| **Event name** | FCAJ Community Meetup |
| **Date** | 06/06/2026 |
| **Location** | <!-- TODO: actual venue (e.g., university auditorium or coworking space, HCMC) --> |
| **Role** | Attendee |
| **Format** | 6 lightning talks (~25 minutes each), spanning Cloud / AI / Career topics |

### Event Objectives

- Give the AWS Vietnam community a stage to share practical knowledge: containers, security, networking, graph, dev career.
- Keep each talk in **lightning-talk format** so attendees get a broad view in a single session.
- Create networking opportunities between current FCAJ interns, alumni, mentors, and guest speakers.

### Speakers

- **Bảo Huỳnh** – Endava Vietnam / ITea Lab.
- **Lê Hoàng Gia Đại** – HUTECH, AWS G3.
- **Nguyễn Quốc Bảo** – Multiplayer cloud engineer.
- **Trương Phước** – Soft skills sharing.
- **Việt Phát** – AWS Neptune + GraphRAG.
- **Vinh Trần** – IT Helpdesk → Senior Sysadmin journey.

### Key Highlights

#### Talk 1 — Docker: A containerization technology (Bảo Huỳnh, Endava Vietnam / ITea Lab)

A clear comparison of **virtualization** vs **containerization**, then a deep dive into Docker.

**Key takeaways:**
- **Virtualization**: each VM has its own OS → heavy, expensive CPU/RAM/storage, requires patching each VM.
- **Containerization**: package app + dependencies → runs consistently anywhere.
- **Container vs VM**: much lighter, fewer resources, ideal for running multiple apps on one host.
- **Dockerfile**: each instruction = one image layer. Unchanged layers → cached, changed layers → rebuilt from there.
- **Use cases**: CI/CD, microservices, dev/test env, cloud-native, legacy modernization.
- The philosophy: **build once, run anywhere**.

**Resources:**
- Slide: `AWS/Meetup 06-06-2026/Bảo Huỳnh - Docker - A containerization technology/Docker_Bảo.pptx`


#### Talk 2 — Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS (Lê Hoàng Gia Đại, HUTECH, AWS G3)

Combining a managed WAF rule-based layer with an **ML-based NIDS** to catch novel attacks.

**Key takeaways:**
- **AWS WAF** protects CloudFront, ALB, API Gateway, Cognito from SQLi, XSS, bot, brute force.
- Rule-based WAF **isn't enough** against novel/zero-day + hybrid attacks.
- **NIDS** (Network Intrusion Detection System) combined with ML learns from network data, detects new patterns.
- **Dataset**: CSE-CIC-IDS2018 (UNB).
- **Pipeline**: Multi-CSV merge → cleaning (invalid label, NaN, ±∞) → balance classes → train (LightGBM).
- **Architecture**: VPC + EC2 + ALB + AWS WAF + S3 + Kinesis Firehose + Lambda + Security Hub + GuardDuty + SNS + CloudWatch.
- **Results**: standardized infra, better admin interface, improved minority class detection through class balancing.
- **Lessons**: data quality determines ML performance; signature alone isn't enough; ML NIDS complements AWS WAF effectively.

**Resources:**
- Slide: `AWS/Meetup 06-06-2026/Lê Hoàng Gia Đại - Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS/AWS WAF & ML NIDS-LeHoangGiaDai.pptx`


#### Talk 3 — Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets (Nguyễn Quốc Bảo)

**Key takeaways (recorded from the live talk):**
- When a **Godot** game client needs realtime multiplayer, the **WebSocket API Gateway** pattern is the simplest way to bridge clients into a shared server session.
- Benefits: low latency, stateless Lambda can fan-out messages, no dedicated game server to operate.
- Real-world challenges: state synchronization, reconnect logic, cost growth when connections increase.

> Note: the slide deck for this talk could not be parsed by python-pptx; the summary above is reconstructed from the live presentation and Q&A. Readers can extend it with their own notes from the session.


#### Talk 4 — Effective teamwork (Trương Phước)

**Key takeaways (recorded from the live talk):**
- Foundations of effective teamwork: **open communication**, **clear ownership**, **commitment to deadlines**.
- Anti-patterns to avoid: silent failures (errors without escalation), decision drift, bus factor = 1.
- Tools suggested: prefer a **shared doc** over fragmented chat channels; short weekly retros; assign a **DRI** (Directly Responsible Individual) to each task.

> Note: the slide deck for this talk could not be parsed; the summary above is reconstructed from the live talk and Q&A.


#### Talk 5 — AWS Neptune for Building a Graph Knowledge Base for GraphRAG (Việt Phát)

**Key takeaways (recorded from the live talk):**
- **Amazon Neptune** is a managed graph database (supports both Property Graph and RDF/W3C).
- Use Neptune as a **knowledge base** for **GraphRAG**: query the relevant subgraph, then feed it into an LLM prompt.
- Advantage over vector-only RAG: better answers for relational questions ("how is A connected to B via C?").
- Lesson: cost scales with nodes/edges — bound the returned subgraph and cache frequent queries.

> Note: the slide deck for this talk could not be parsed; the summary above is reconstructed from the live talk and Q&A.


#### Talk 6 — From IT Helpdesk to Senior Sysadmin: Self-learning Journey and Path to Cloud-DevOps (Vinh Trần)

**Key takeaways (recorded from the live talk):**
- A realistic career path: **IT Helpdesk → Junior Sysadmin → Sysadmin → Senior Sysadmin**, then branching into **Cloud / DevOps** once the OS, networking, and troubleshooting foundation is solid.
- Suggested self-learning path:
  - **Linux fundamentals** (LPIC-1) + bash scripting.
  - **Networking**: TCP/IP, DNS, HTTP, firewalls.
  - **AWS**: Solutions Architect Associate first, then SysOps Administrator.
  - **IaC**: Terraform, then Ansible.
- Mindset: every helpdesk ticket is a lesson about **how systems fail** — don't just fix, dig to the root cause.

> Note: the slide deck for this talk could not be parsed; the summary above is reconstructed from the live talk and Q&A.

### What I learned from this event

- **Containerization** differs from virtualization by **sharing the kernel** — that's why Docker is much lighter than VMs. Useful mental model when weighing SageMaker training jobs (managed container) vs self-managed EC2 for the capstone.
- **AWS WAF + ML NIDS** is a strong pattern: managed rule-based layer + custom ML for unknown threats. I learned to separate the **signature** layer (fixed rules) from the **statistical** layer (ML) when designing **drift detection**: baseline statistics for "known-good", PSI/KL divergence for "unknown drift".
- Some talks had slides I couldn't read (file parse errors), yet reconstructing notes from the live talk and Q&A still gave me **a sufficiently broad view** — a lesson in **not depending 100% on slides** during events.
- **Vinh's Helpdesk → Cloud-DevOps** story is motivating: you don't need to start at "Cloud Engineer" right away; what matters is a **solid systems foundation**.

### Applying to my capstone

- Split monitoring into two clear layers: **rule-based** (fixed CloudWatch alarms) + **statistical** (PSI/KL divergence for drift). Exactly the "managed layer + custom ML layer" mindset from talk #2.
- When designing drift detection, I borrow talk #5's idea of bounding query cost: limit the captured-request batch size per hour, cache baseline statistics.
- **Experiment small before scaling**: like Docker's image layer caching, I split preprocessing → training → HPO into independent steps; unchanged layers don't need to re-run.



> Overall, the meetup broadened my view beyond just MLOps: containers, security, graphs, networking, sysadmin — all essential pieces when running ML systems at production-grade.

### References

* Slide decks in `AWS/Meetup 06-06-2026/` (4 of 6 files failed to parse via python-pptx; talk summaries are reconstructed from the live presentations and Q&A).
