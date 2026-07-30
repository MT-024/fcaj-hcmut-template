---
title: "Worklog Tuần 1"
date: 2026-06-01
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
reportTableColumns:
  - Thứ
  - Công việc
  - Ngày hoàn thành
reportType: worklog
---

### Mục tiêu tuần 1:

* Làm quen với các thành viên First Cloud AI Journey (FCAJ), nắm nội quy chương trình và mentor phụ trách.
* Làm quen AWS Console, AWS CLI và region `ap-southeast-1` sẽ dùng xuyên suốt 8 tuần.
* Đọc brief đồ án SageMaker MLOps capstone và xác định ràng buộc cứng: budget cap 200 USD, single region, no GPU, no NAT Gateway.

### Công việc cần làm trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| --- | --------- | ------------ | --------------- |
| 2   | - Làm quen các thành viên FCAJ và mentor <br> - Đọc nội quy, quy định tại đơn vị thực tập <br> - Xác nhận kênh liên lạc (Slack, email, lịch weekly meeting) | 01/06/2026 | 01/06/2026 |
| 3   | - Tạo AWS Free Tier account <br> - Cài đặt và cấu hình AWS CLI (`aws configure`) trỏ region `ap-southeast-1` <br> - Verify `aws sts get-caller-identity` trả về đúng account | 02/06/2026 | 02/06/2026 |
| 4   | - Tìm hiểu AWS Console: VPC, S3, IAM, SageMaker, Lambda, API Gateway, CloudWatch <br> - Phân biệt managed service vs serverless vs container | 03/06/2026 | 03/06/2026 |
| 5   | - Đọc kỹ brief SageMaker MLOps capstone: scope 8 tuần, dataset heart-attack-risk, success metrics, deliverable cuối <br> - Đọc qua `CLAUDE.md` để nắm schema data, processed splits 80/10/10, brief constraints | 04/06/2026 | 04/06/2026 |
| 6   | - Liệt kê AWS services cần dùng → ước lượng chi phí theo bảng giá AWS <br> - Xác nhận lại với mentor budget cap 200 USD, single region, không GPU, không NAT Gateway | 05/06/2026 | 05/06/2026 |

### Thành quả tuần 1:

* Đã onboard vào chương trình FCAJ, biết mentor và lịch sinh hoạt hàng tuần.
* AWS Free Tier account + CLI sẵn sàng, region `ap-southeast-1` đã cố định cho cả đồ án.
* Nắm rõ brief SageMaker MLOps: scope, dataset, metrics mục tiêu, deliverable.
* Ước lượng sơ bộ chi phí cho thấy budget 200 USD khả thi với cấu hình dự kiến (không GPU, serverless inference).
