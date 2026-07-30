---
title: "Tuần 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Thứ
  - Công việc
  - Ngày hoàn thành
reportHeadings:
  - Mục tiêu tuần 2
  - Công việc cần làm trong tuần
  - Thành quả tuần 2
---
{{% notice warning %}}
⚠️ **Lưu ý:** Thông tin dưới đây chỉ mang tính tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn.
{{% /notice %}}


### Mục tiêu tuần 2:

* Hiểu schema dataset heart-attack (raw + processed) và các ràng buộc chất lượng dữ liệu từ brief.
* Xây pipeline tiền xử lý sklearn tái sử dụng được, fit chỉ trên train (không leakage).
* Làm quen với AWS console: tạo S3 bucket, IAM role cho SageMaker, cấu hình region `ap-southeast-1`.


### Công việc cần làm trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| --- | --------- | ------------ | --------------- |
| 2   | - Đọc kỹ brief `Nội dung.docx`: kế hoạch 8 tuần, budget cap 200 USD, IAM scope, S3 paths <br> - Đọc qua `CLAUDE.md` (schema data, processed splits 80/10/10) | 12/08/2025 | 12/08/2025 |
| 3   | - Khảo sát `raw/heart_attack_dataset.csv` (7000 dòng × 22 cột) <br> - Xác định cột có missing values (smoking_status, cholesterol, oldpeak, ...) <br> - Verify schema: age 18–100, fasting_blood_sugar ∈ {0,1}, num_major_vessels 0–3 | 13/08/2025 | 13/08/2025 |
| 4   | - Viết `preprocess.py` với sklearn ColumnTransformer: StandardScaler cho numeric, OneHotEncoder cho nominal, OrdinalEncoder cho ordinal, passthrough cho binary <br> - Fit CHỈ trên train split để tránh leakage | 14/08/2025 | 15/08/2025 |
| 5   | - Sinh `processed/{train,val,test}_processed.csv` <br> - Verify prefix cột: `num__`, `norm_num__`, `bin__`, `nom__`, `ord__` <br> - Lưu `preprocessor.joblib` đã fit để dùng lại | 15/08/2025 | 16/08/2025 |
| 6   | - Tạo S3 bucket `s3://heart-risk-mlops-<account-id>/` ở `ap-southeast-1` <br> - Tạo SageMaker execution IAM role least-privilege (chỉ S3 + logs + SageMaker + PassRole) <br> - Thêm lifecycle rule: logs/artifacts hết hạn sau 30 ngày | 16/08/2025 | 16/08/2025 |


### Thành quả tuần 2:

* Nắm rõ ràng buộc brief: budget cap 200 USD, single region, no GPU, no NAT Gateway.
* Xây xong pipeline tiền xử lý sklearn (ColumnTransformer) với 5 nhóm transformer, fit trên train only.
* Sinh 3 file processed (5600/700/700 dòng) với prefix cột đúng chuẩn.
* Khởi tạo S3 bucket + IAM role với lifecycle rule — biện pháp tiết kiệm chi phí đầu tiên.


### Tài liệu tham khảo:

* https://scikit-learn.org/stable/modules/compose.html#columntransformer-for-heterogeneous-data
* https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html