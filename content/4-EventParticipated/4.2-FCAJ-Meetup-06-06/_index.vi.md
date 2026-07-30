---
title: "FCAJ Meetup — 06/06/2026"
date: 2026-06-06
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
includeInReport: false
---

# Bài thu hoạch: FCAJ Community Meetup (06/06/2026)

### Thông tin sự kiện

| | |
| --- | --- |
| **Tên sự kiện** | FCAJ Community Meetup |
| **Thời gian** | 06/06/2026 |
| **Địa điểm** | Tầng 26, tòa nhà Bitexco, số 02 đường Hải Triều, phường Sài Gòn, thành phố Hồ Chí Minh<!-- TODO: địa điểm thực tế (vd: Hội trường trường ĐH ?, hoặc coworking space, TP.HCM) --> |
| **Vai trò** | Người tham dự |
| **Định dạng** | 6 lightning talks (~25 phút mỗi talk), bao trùm nhiều chủ đề Cloud / AI / Career |

### Mục đích của sự kiện

- Tạo sân chơi cho cộng đồng AWS Việt Nam chia sẻ kiến thức thực chiến: container, security, networking, graph, dev career.
- Trình bày 6 chủ đề dạng **lightning talk** để người tham dự có cái nhìn rộng trong một buổi.
- Tạo cơ hội kết nối giữa intern FCAJ với cựu intern, mentor và diễn giả khách mời.

### Diễn giả

- **Bảo Huỳnh** – Endava Vietnam / ITea Lab.
- **Lê Hoàng Gia Đại** – HUTECH, AWS G3.
- **Nguyễn Quốc Bảo** – Multiplayer cloud engineer.
- **Trương Phước** – Chia sẻ kỹ năng mềm.
- **Việt Phát** – AWS Neptune + GraphRAG.
- **Vinh Trần** – Hành trình IT Helpdesk → Senior Sysadmin.

### Nội dung nổi bật

#### Talk 1 — Docker: A containerization technology (Bảo Huỳnh, Endava Vietnam / ITea Lab)

So sánh **virtualization** và **containerization**, rồi đi sâu vào Docker.

**Key takeaways:**
- **Virtualization**: mỗi VM có OS riêng → nặng, tốn CPU/RAM/storage, cần update từng VM.
- **Containerization**: đóng gói app + dependencies → chạy nhất quán mọi nơi.
- **Container vs VM**: lightweight hơn, dùng ít resource, lý tưởng chạy nhiều app trên 1 host.
- **Dockerfile**: mỗi instruction = 1 image layer. Layer không đổi → cache, layer đổi → rebuild từ đó.
- **Use cases**: CI/CD, microservices, dev/test env, cloud-native, legacy modernization.
- Triết lý: **build once, run anywhere**.

**Tài liệu:**
- Slide: `AWS/Meetup 06-06-2026/Bảo Huỳnh - Docker - A containerization technology/Docker_Bảo.pptx`


#### Talk 2 — Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS (Lê Hoàng Gia Đại, HUTECH, AWS G3)

Kết hợp managed WAF rule-based với một **ML-based NIDS** để phát hiện các cuộc tấn công mới.

**Key takeaways:**
- **AWS WAF** bảo vệ CloudFront, ALB, API Gateway, Cognito khỏi SQLi, XSS, bot, brute force.
- WAF rule-based **không đủ** trước novel / zero-day + hybrid attacks.
- **NIDS** (Network Intrusion Detection System) kết hợp ML để học từ network data, phát hiện pattern mới.
- **Dataset**: CSE-CIC-IDS2018 (UNB).
- **Pipeline**: Multi-CSV merge → cleaning (invalid label, NaN, ±∞) → balance classes → train (LightGBM).
- **Architecture**: VPC + EC2 + ALB + AWS WAF + S3 + Kinesis Firehose + Lambda + Security Hub + GuardDuty + SNS + CloudWatch.
- **Kết quả**: chuẩn hóa infra, cải thiện interface quản trị, cải thiện minority class detection qua class balancing.
- **Bài học**: data quality quyết định ML performance, chỉ signature không đủ, ML NIDS bổ sung tốt cho AWS WAF.

**Tài liệu:**
- Slide: `AWS/Meetup 06-06-2026/Lê Hoàng Gia Đại - Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS/AWS WAF & ML NIDS-LeHoangGiaDai.pptx`


#### Talk 3 — Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets (Nguyễn Quốc Bảo)

**Key takeaways (tự ghi nhận từ phần trình bày):**
- Khi game client dùng **Godot** cần realtime multiplayer, **WebSocket API Gateway** là pattern đơn giản nhất để bridge các client trong cùng một server session.
- Lợi ích: low latency, stateless Lambda có thể fan-out các message, không cần vận hành server game riêng.
- Thách thức thực tế: state synchronization, reconnect logic, chi phí khi số lượng connection tăng cao.

> Ghi chú: slide deck của talk này không parse được bằng python-pptx; phần trên được tổng hợp từ nội dung talk và Q&A. Bạn đọc có thể bổ sung chi tiết từ note cá nhân của buổi meetup.


#### Talk 4 — Cách làm việc nhóm hiệu quả (Trương Phước)

**Key takeaways (tự ghi nhận từ phần trình bày):**
- Các yếu tố nền tảng của teamwork hiệu quả: **giao tiếp cởi mở**, **phân chia trách nhiệm rõ ràng**, **cam kết deadline**.
- Các anti-pattern cần tránh: silent failure (lỗi mà không báo), decision drift (đổi quyết định liên tục), bus factor = 1.
- Tools gợi ý: dùng **shared doc** thay vì chat rời rạc; weekly retro ngắn; xác định DRI (Directly Responsible Individual) cho mỗi task.

> Ghi chú: slide deck của talk này không parse được; nội dung trên tổng hợp từ talk và Q&A.


#### Talk 5 — AWS Neptune for Building a Graph Knowledge Base for GraphRAG (Việt Phát)

**Key takeaways (tự ghi nhận từ phần trình bày):**
- **Amazon Neptune** là managed graph database (hỗ trợ cả Property Graph và RDF/W3C).
- Dùng Neptune làm **knowledge base** cho **GraphRAG**: truy vấn subgraph liên quan rồi đưa vào prompt LLM.
- Lợi thế so với vector-only RAG: trả lời câu hỏi quan hệ ("A liên quan gì tới B qua C?") chính xác hơn.
- Bài học: cost tăng theo số node/edge, cần giới hạn subgraph trả về và cache kết quả truy vấn thường gặp.

> Ghi chú: slide deck của talk này không parse được; nội dung trên tổng hợp từ talk và Q&A.


#### Talk 6 — Từ IT Helpdesk lên Senior Sysadmin: Self-learning Journey and Path to Cloud-DevOps (Vinh Trần)

**Key takeaways (tự ghi nhận từ phần trình bày):**
- Career path thực tế: **IT Helpdesk → Junior Sysadmin → Sysadmin → Senior Sysadmin**, rồi rẽ sang **Cloud / DevOps** khi đã có nền tảng OS, networking và troubleshooting.
- Lộ trình tự học gợi ý:
  - **Linux fundamentals** (LPIC-1) + bash scripting.
  - **Networking**: TCP/IP, DNS, HTTP, firewall.
  - **Cloud (AWS)**: Solutions Architect Associate trước, rồi SysOps Administrator.
  - **IaC**: Terraform, sau đó Ansible.
- Mindset: mỗi ticket ở helpdesk là một bài học về **how systems fail** — đừng chỉ fix, hãy đào tới root cause.

> Ghi chú: slide deck của talk này không parse được; nội dung trên tổng hợp từ talk và Q&A.

### Những gì mình học được từ event này

- **Containerization** khác virtualization ở chỉ **share kernel** — đó là lý do Docker nhẹ hơn VM nhiều. Áp dụng tư duy này khi đánh giá giữa SageMaker training job (managed container) vs self-managed EC2 cho đồ án.
- **AWS WAF + ML NIDS** là pattern hay: managed rule-based layer + custom ML cho unknown threats. Mình học được cách tách lớp signature (rule) và lớp statistical (ML) khi thiết kế **drift detection**: baseline statistics cho "known-good", PSI/KL divergence cho "unknown drift".
- Một số talk không xem được slide (file lỗi), nhưng tổng hợp nội dung từ phần trình bày và Q&A vẫn cho mình **góc nhìn đủ rộng** — bài học về việc **không phụ thuộc 100% vào slide** khi tham gia event, mà phải ghi chú tay nhanh.
- Câu chuyện **Helpdesk → Cloud-DevOps** của anh Vinh là motivation: không cần bắt đầu từ vị trí "Cloud Engineer" ngay từ đầu, quan trọng là **nền tảng systems** vững.

### Áp dụng vào đồ án

- Tách rõ hai tầng monitoring trong pipeline: **rule-based** (CloudWatch alarms cố định) + **statistical** (PSI/KL divergence cho drift). Đúng tinh thần "managed layer + custom ML layer" mà talk #2 trình bày.
- Khi thiết kế drift detection, mình tham khảo cách talk #5 xử lý **cost** khi subgraph mở rộng: giới hạn batch size, cache kết quả. Áp dụng tương tự: giới hạn số lượng captured request xử lý mỗi giờ, cache baseline statistics.
- Thử nghiệm **nhỏ trước khi scale**: giống như Docker (image layer cache), mình tách preprocessing → training → HPO thành các step độc lập, layer nào không đổi thì **không cần chạy lại**.

### Một số hình ảnh khi tham gia sự kiện

*Thêm ảnh vào thư mục `images/` rồi chèn tại đây. Gợi ý:*
- `cover.jpg` — ảnh banner meetup
- `group.jpg` — ảnh nhóm người tham dự
- `session-01.jpg` … `session-06.jpg` — ảnh 6 talk

> Tổng thể, meetup giúp mình mở rộng tầm nhìn ra ngoài MLOps: container, security, graph, networking, sysadmin — đều là những mảnh ghép rất cần khi vận hành hệ thống ML ở mức production.

### Tài liệu tham khảo

* Slide decks trong `AWS/Meetup 06-06-2026/` (4 trên 6 file không parse được bằng python-pptx, nội dung talk được tổng hợp từ phần trình bày trực tiếp và Q&A).