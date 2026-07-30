---
title: "FCAJ Meetup — 06/06/2026"
date: 2026-06-06
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
includeInReport: false
---
{{% notice info %}}
**Event:** FCAJ community meetup
**Date:** 06/06/2026
**Format:** 6 lightning talks (~25 minutes each)
{{% /notice %}}


### Talks at this meetup

#### 1. Docker — A containerization technology — Bảo Huỳnh (Endava Vietnam, ITea Lab)

**Key takeaways:**
- **Virtualization:** each VM has its own OS → heavy, expensive CPU/RAM/storage, requires patching each VM.
- **Containerization:** package app + dependencies → runs consistently anywhere.
- **Container vs VM:** much lighter, fewer resources, ideal for running multiple apps on one host.
- **Dockerfile:** each instruction = one image layer. Unchanged layers → cached, changed layers → rebuilt from there.
- **Use cases:** CI/CD, microservices, dev/test env, cloud-native, legacy modernization.
- Docker: build once, run anywhere.

**Resources:**
- Slide: `AWS/Meetup 06-06-2026/Bảo Huỳnh - Docker - A containerization technology/Docker_Bảo.pptx`


#### 2. Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS — Lê Hoàng Gia Đại (HUTECH, AWS G3)

**Key takeaways:**
- **AWS WAF** protects CloudFront, ALB, API Gateway, Cognito from SQLi, XSS, bot, brute force.
- Rule-based WAF **isn't enough** against novel/zero-day + hybrid attacks.
- **NIDS** (Network Intrusion Detection System) combined with ML learns from network data, detects new patterns.
- **Dataset:** CSE-CIC-IDS2018 (UNB).
- **Pipeline:** Multi-CSV merge → cleaning (invalid label, NaN, ±∞) → balance classes → train (LightGBM).
- **Architecture:** VPC + EC2 + ALB + AWS WAF + S3 + Kinesis Firehose + Lambda + Security Hub + GuardDuty + SNS + CloudWatch.
- **Results:** standardized infra, better admin interface, improved minority class detection through class balancing.
- **Lessons:** data quality determines ML performance; signature alone isn't enough; ML NIDS complements AWS WAF effectively.

**Resources:**
- Slide: `AWS/Meetup 06-06-2026/Lê Hoàng Gia Đại - Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS/AWS WAF & ML NIDS-LeHoangGiaDai.pptx`


#### 3. Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets — Nguyễn Quốc Bảo

(Slide deck could not be parsed — speaker + topic only recorded.)


#### 4. Cách làm việc nhóm hiệu quả — Trương Phước

(Slide deck could not be parsed — speaker + topic only recorded.)


#### 5. AWS Neptune for Building a Graph Knowledge Base for GraphRAG — Việt Phát

(Slide deck could not be parsed — speaker + topic only recorded.)


#### 6. Từ IT Helpdesk lên Senior Sysadmin: Self-learning Journey and Path to Cloud-DevOps — Vinh Trần

(Slide deck could not be parsed — speaker + topic only recorded.)


### What I learned from this event

- Containerization differs from virtualization by sharing the kernel — that's why Docker is much lighter than VM.
- AWS WAF + ML NIDS is a nice pattern: managed rule-based layer + custom ML for unknown threats.
- Many talks had slides I couldn't read (file parse errors), but the speaker + topic are recorded for follow-up.


### References

* Slide decks in `AWS/Meetup 06-06-2026/` (4 of 6 files failed to parse via python-pptx)