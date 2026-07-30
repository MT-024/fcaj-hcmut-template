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
**Format:** 6 lightning talks (~25 phút mỗi talk)
{{% /notice %}}


### Talks tại meetup này

#### 1. Docker — A containerization technology — Bảo Huỳnh (Endava Vietnam, ITea Lab)

**Key takeaways:**
- **Virtualization:** mỗi VM có OS riêng → nặng, tốn CPU/RAM/storage, cần update từng VM.
- **Containerization:** đóng gói app + dependencies → chạy nhất quán mọi nơi.
- **Container vs VM:** lightweight hơn, dùng ít resource, lý tưởng chạy nhiều app trên 1 host.
- **Dockerfile:** mỗi instruction = 1 image layer. Layer không đổi → cache, layer đổi → rebuild từ đó.
- **Use cases:** CI/CD, microservices, dev/test env, cloud-native, legacy modernization.
- Docker build once, run anywhere.

**Tài liệu:**
- Slide: `AWS/Meetup 06-06-2026/Bảo Huỳnh - Docker - A containerization technology/Docker_Bảo.pptx`


#### 2. Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS — Lê Hoàng Gia Đại (HUTECH, AWS G3)

**Key takeaways:**
- **AWS WAF** bảo vệ CloudFront, ALB, API Gateway, Cognito khỏi SQLi, XSS, bot, brute force.
- WAF rule-based **không đủ** trước novel/zero-day + hybrid attacks.
- **NIDS** (Network Intrusion Detection System) kết hợp ML để học từ network data, phát hiện pattern mới.
- **Dataset:** CSE-CIC-IDS2018 (UNB).
- **Pipeline:** Multi-CSV merge → cleaning (invalid label, NaN, ±∞) → balance classes → train (LightGBM).
- **Architecture:** VPC + EC2 + ALB + AWS WAF + S3 + Kinesis Firehose + Lambda + Security Hub + GuardDuty + SNS + CloudWatch.
- **Kết quả:** chuẩn hóa infra, cải thiện interface quản trị, cải thiện minority class detection qua class balancing.
- **Bài học:** data quality quyết định ML performance, chỉ signature không đủ, ML NIDS bổ sung tốt cho AWS WAF.

**Tài liệu:**
- Slide: `AWS/Meetup 06-06-2026/Lê Hoàng Gia Đại - Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS/AWS WAF & ML NIDS-LeHoangGiaDai.pptx`


#### 3. Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets — Nguyễn Quốc Bảo

(Slide deck không parse được — chỉ ghi nhận speaker + topic.)


#### 4. Cách làm việc nhóm hiệu quả — Trương Phước

(Slide deck không parse được — chỉ ghi nhận speaker + topic.)


#### 5. AWS Neptune for Building a Graph Knowledge Base for GraphRAG — Việt Phát

(Slide deck không parse được — chỉ ghi nhận speaker + topic.)


#### 6. Từ IT Helpdesk lên Senior Sysadmin: Hành trình tự học và lộ trình dịch chuyển sang Cloud-DevOps — Vinh Trần

(Slide deck không parse được — chỉ ghi nhận speaker + topic.)


### Những gì mình học được từ event này

- Containerization khác virtualization ở chỗ share kernel — đó là lý do Docker nhẹ hơn VM nhiều.
- AWS WAF + ML NIDS là pattern hay: managed rule-based layer + custom ML cho unknown threats.
- Nhiều talk mình không xem được slide (do file lỗi), nhưng speaker + topic đã được ghi nhận để follow-up.


### Tài liệu tham khảo

* Slide decks trong `AWS/Meetup 06-06-2026/` (4 trên 6 file không parse được bằng python-pptx)