---
title: "Tuần 8"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Thứ
  - Công việc
  - Ngày hoàn thành
reportHeadings:
  - Mục tiêu tuần 8
  - Công việc cần làm trong tuần
  - Thành quả tuần 8
---
{{% notice warning %}}
⚠️ **Lưu ý:** Chỉ mang tính tham khảo.
{{% /notice %}}


### Mục tiêu tuần 8:

* Orchestrate full pipeline với SageMaker Pipelines.
* Chạy cleanup script để xóa toàn bộ AWS resources.
* Verify tổng cost giữ dưới 200 USD.


### Công việc cần làm trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| --- | --------- | ------------ | --------------- |
| 2   | - Định nghĩa SageMaker Pipeline (Kubeflow-based): ProcessingStep → TrainingStep → HPOStep → RegisterModelStep → DeployStep | 23/09/2025 | 24/09/2025 |
| 3   | - Test pipeline end-to-end trên batch nhỏ (1 trial thay vì 6 cho nhanh) <br> - Verify pipeline ARN, execution ARN, status | 24/09/2025 | 25/09/2025 |
| 4   | - Chạy lại với HPO đầy đủ (6 trials) — final pipeline execution | 25/09/2025 | 26/09/2025 |
| 5   | - Viết cleanup script `cleanup.py`: delete endpoint, delete model package, delete pipeline, empty S3 bucket (sau khi download) <br> - Filter theo tag `Project=heart-risk-mlops` để tránh đụng resource khác | 26/09/2025 | 27/09/2025 |
| 6   | - Chạy cleanup, verify mọi resource đã xóa qua Cost Explorer <br> - Đối chiếu bill cuối với cap 200 USD | 27/09/2025 | 27/09/2025 |


### Thành quả tuần 8:

* End-to-end pipeline reproduce được từ một lệnh `pipeline.start()`.
* Toàn bộ AWS resource đã cleanup qua script (kỷ luật chi phí).
* Project delivered trong budget.

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Tuần 8 - Pipeline + Cleanup                   -->
<!-- Cần điền:                                                       -->
<!--   - Pipeline ARN                                                -->
<!--   - Total project cost (bill cuối, vd: 87 USD)                  -->
<!--   - Cost breakdown theo service                                 -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->


### Tài liệu tham khảo:

* https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines.html