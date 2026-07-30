---
title: "Workshop"
date: 2026-06-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
includeInReport: false
---

# Dự đoán nguy cơ đau tim — Workshop MLOps End-to-End

#### Tổng quan

Workshop này là nhật ký xây dựng đồ án **SageMaker MLOps capstone** của em trong kỳ thực tập FCAJ. Tài liệu đi qua từng bước từ một file CSV thô trên S3 đến một REST API đã triển khai và có giám sát trên AWS, với ngân sách giới hạn **200 USD**, một region duy nhất (`ap-southeast-1`), không dùng GPU và không dùng NAT Gateway.

Workshop không phải một bài lab để bạn đọc tự làm theo; đây là tài liệu mô tả **cách em đã dựng hệ thống**: những gì đã được provision, từng bước làm gì, kết quả trông ra sao và kỷ luật chi phí nằm ở đâu. Mỗi trang con tương ứng với một giai đoạn của bản dựng, đúng thứ tự với 8 tuần thực tập.

![Kiến trúc Heart-Attack-Risk Prediction](/images/2-Proposal/aws-flow.jpg)

#### Nội dung

1. [Tổng quan workshop](5.1-Workshop-overview/)
2. [Chuẩn bị — AWS account, IAM, Budgets](5.2-Prerequiste/)
3. [Tiền xử lý dữ liệu trên SageMaker (Tuần 2)](5.3-S3-vpc/)
4. [Huấn luyện, HPO, endpoint và API (Tuần 3–6)](5.4-S3-onprem/)
5. [Phát hiện drift và CloudWatch alarm (Tuần 7)](5.5-Policy/)
6. [SageMaker Pipeline và cleanup (Tuần 8)](5.6-Cleanup/)