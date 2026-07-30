---
title: "Chia sẻ và Phản hồi"
date: 2026-07-30
weight: 7
chapter: false
pre: " <b> 7. </b> "
includeInReport: false
---

> Trong phần này, em chia sẻ góc nhìn cá nhân về trải nghiệm tham gia chương trình First Cloud AI Journey xuyên suốt 8 tuần đồ án SageMaker MLOps. Những ý kiến này nhằm giúp đội ngũ FCAJ cải thiện các điểm còn hạn chế cho các khóa sau.

### Đánh giá tổng thể

**1. Môi trường làm việc**
Môi trường làm việc tại FCAJ hỗ trợ và thực tế. Mỗi khi một bước pipeline bị kẹt (cấu hình Data Capture sai, job thống kê baseline của drift), em đều có thể nhắn mentor trên Slack và được gỡ vướng trong vài phút. Buổi review hàng tuần giúp em chịu trách nhiệm với budget cap 200 USD và buộc phải công khai các đánh đổi. Các kênh cộng đồng AWS là nơi em có thể đặt câu hỏi kiến trúc vượt ra ngoài bandwidth của mentor. Cho các khóa sau, sẽ tốt hơn nếu có một **sổ lab chung** (hoặc kênh Slack được pin) nơi các intern đăng log 1 dòng mỗi ngày, để các bạn cùng khóa phát hiện blocker sớm hơn.

**2. Mức liên quan tới chuyên ngành**
Đồ án nằm đúng vào chỗ mà chương trình học để lại: em đã học XGBoost, tiền xử lý, và khái niệm cloud cơ bản ở trường, nhưng chưa bao giờ ghép chúng thành một pipeline MLOps end-to-end trên AWS. SageMaker Processing → Training → HPO → Model Registry → Serverless Endpoint → Data Capture → Drift Detection chính là lớp tích hợp còn thiếu. Dataset (dự đoán nguy cơ đau tim, 7000 dòng) đủ nhỏ để giữ trong cap 200 USD nhưng đủ thực tế để các quyết định chất lượng dữ liệu — class balance, xử lý missing value, validate schema — thực sự có ý nghĩa. Đây là thiết lập gần với production hơn bất kỳ bài tập lớn nào em đã làm ở trường.

**3. Cơ hội học tập & phát triển kỹ năng**
Những kỹ năng cụ thể em học được ngoài giáo trình:
- **SageMaker MLOps pipeline as code**: ProcessingStep / TrainingStep / HPOStep / RegisterModelStep nối qua Kubeflow, chạy end-to-end bằng một `pipeline.start()`.
- **Cost engineering**: Spot Training, Serverless Inference, lifecycle rule cho logs/artifacts, `max_parallel_jobs=1` cho HPO — và kỷ luật **xóa endpoint giữa các demo** để tránh idle bill ~35 USD/tháng.
- **Drift detection không dùng managed Model Monitor**: Data Capture → S3 → EventBridge (rule 1 giờ) → Processing Job → PSI/KL divergence → CloudWatch custom metric → SNS alarm. Tự dựng bằng tay buộc em hiểu từng hop.
- **API hardening**: Lambda với IAM scope least-privilege (chỉ `sagemaker:InvokeEndpoint` trên endpoint ARN), API Gateway AWS_PROXY integration, disclaimer cố định trên mọi response để demo trung thực.
- **Technical writing**: 3 bài blog đã đăng trên cộng đồng AWS Việt Nam, mỗi bài được mentor review trước khi xuất bản.

**5. Văn hóa công ty & tinh thần đồng đội**
Tinh thần **"No-Blame Post-Mortem"** mà diễn giả MNC chia sẻ ở FCAJ Meet 13/06/2026 cũng chính là cách đội FCAJ vận hành. Khi HPO job fail ở trial thứ 3 vì mình over-spec search space, cuộc thảo luận là *"failure này nói gì về search space"* chứ không phải *"sao không check trước"*. Văn hóa đó khiến mình an tâm để đưa ra các quyết định mạo hiểm về chi phí (real-time endpoint? GPU instance?) rồi walk back khi số liệu không khớp. Phòng team ở tuần hackathon FCAJ x AABW cũng cho thấy cùng vibe đó: giúp đỡ chéo team thay vì cạnh tranh nội bộ.

**6. Chính sách & phúc lợi thực tập**
Budget 200 USD cho AWS là mức constraint hợp lý — đủ để chạy thật pipeline end-to-end, đủ chặt để mình phải nghĩ về cost ở mỗi bước. Kênh Slack cảnh báo budget và các ràng buộc cứng trong brief (single region `ap-southeast-1`, không GPU, không NAT Gateway) giúp mình tránh được các quyết định overspend trước khi chúng xảy ra. Một thứ mình ước có: **template cost guardrail dựng sẵn** (CloudFormation hoặc CDK) wire up sẵn ở tuần 1, để mình dành tuần 2–3 cho pipeline thay vì tự dựng lại AWS Budgets + SNS.

---

### Câu hỏi bổ sung

- **Điều gì khiến bạn hài lòng nhất trong kỳ thực tập?**
  Thiết kế pipeline drift detection như một fallback cho Model Monitor — cụ thể là khoảnh khắc CloudWatch alarm thực sự fire khi mình gửi drifted traffic tới endpoint. Nhìn chuỗi alert mình tự dựng bằng tay (Data Capture → EventBridge → Processing Job → PSI → CloudWatch → SNS) sáng đèn end-to-end là khoảnh khắc kỹ thuật thỏa mãn nhất của 8 tuần. Thứ hai: đến tuần 8 và thấy tổng bill dưới 200 USD sau khi cleanup script chạy xong.

- **Bạn nghĩ công ty nên cải thiện gì cho các intern khoá sau?**
  Ba điều cụ thể:
  1. **Ship template cost guardrail dựng sẵn ở tuần 1**, để intern không tự dựng lại AWS Budgets + SNS. Brief cũng nên có sẵn một `cleanup.py` mẫu để tuần cuối không phải mất thời gian khám phá lại resource nào cần xóa.
  2. **Thêm một tuần riêng cho "production-readiness"** (hoặc stretch goal). Endpoint security (VPC, IAM scope tightening), observability (CloudWatch alarms trên các metric đúng, không phải tất cả), và cost dashboard cho mentor review. Những phần này bị nhồi vào 2 tuần cuối của mình.
  3. **Đẩy milestone blog đầu tiên về tuần 4**, không phải tuần cuối. Viết buộc tổng hợp sớm, và viết sớm sẽ phát hiện gap trong giả định pipeline.

- **Nếu giới thiệu cho bạn bè, bạn có khuyên họ thực tập ở đây không? Tại sao?**
  Có — nhưng có ngữ cảnh. Khuyên FCAJ nếu bạn muốn **end-to-end ownership** một hệ thống ML thật dưới ràng buộc budget thật, và nếu bạn sẵn sàng viết về những gì mình học (cadence blog là bắt buộc, không tùy chọn). Không khuyên nếu bạn muốn được spoon-feed một tutorial — chương trình kỳ vọng bạn tự đọc AWS docs, tự debug job, và đặt câu hỏi có mục tiêu cho mentor, không phải "em làm tiếp theo là gì".

---

### Đề xuất & kỳ vọng

- **Đề xuất cải thiện trải nghiệm thực tập**:
  - Cung cấp sample `ap-southeast-1` CloudFormation/CDK template ở tuần 1 cho phần AWS account scaffolding (S3 bucket + IAM role + lifecycle rule + Budget alarm + SNS topic). Sẽ cắt khoảng 2 ngày trial-and-error.
  - Pair intern theo chủ đề quan tâm ở tuần 1 — chỉ cần 30 phút "buddy" pairing cũng giúp được khi mentor không online.
  - Làm cho thư viện slide-deck (từ các meetup trước) có thể search theo topic. Vài slide-deck của meetup mình không parse được bằng python-pptx, và mình phải tổng hợp takeaways từ note.
  - Khuyến khích (hoặc yêu cầu) xuất bản blog **trước tuần 8**. Viết dưới deadline ở tuần cuối sẽ giảm chiều sâu.

- **Bạn có muốn tiếp tục chương trình này trong tương lai không?**
  Có — mình sẽ tham gia một khóa nối tiếp tập trung vào **production hardening** (multi-AZ failover, IaC refactor, canary deployment cho model version mới). 8 tuần capstone cho mình một pipeline MLOps chạy được; lớp kỹ năng tiếp theo là làm cho nó sống sót qua traffic thật.

- **Bình luận khác (chia sẻ tự do)**:
  Điều lớn nhất mình đánh giá thấp là **bao nhiêu phần của MLOps là plumbing không hấp dẫn**: IAM scope, lifecycle rule, log retention, endpoint cleanup, job thống kê baseline drift chạy đồng bộ mất 20 phút. Brief đồ án trung thực về điều này, nhưng mình muốn flag cho intern khoá sau: sự thỏa mãn đến từ toàn hệ thống chạy được, không phải từ một component nào đó. Và nếu bạn chỉ có thời gian đọc một AWS doc end-to-end, hãy đọc **SageMaker Pipelines developer guide** — nó gói gọn Processing, Training, HPO, Registry, Deploy vào cùng một mental model.

  Cảm ơn các mentor FCAJ và cộng đồng AWS Việt Nam đã đồng hành 8 tuần — và đội FCAJ x AABW hackathon đã cho thấy một tuần build có thể ra sản phẩm thật.
