---
title: "Tiền xử lý dữ liệu (Tuần 2)"
date: 2026-06-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

Mục tiêu tuần 2 là lấy file CSV thô 7,000 dòng đang nằm trong S3 và biến nó thành ba tập `train / validation / test` sạch cùng một preprocessor đã fit — tất cả trên hạ tầng managed, không phụ thuộc trạng thái notebook ở local.

Bước xử lý được biểu diễn dưới dạng một SageMaker **Processing Job** đọc `raw/heart_attack_risk.csv` từ bucket dự án, validate schema, impute missing values, encode feature phân loại, chia 70/15/15 có stratification, fit preprocessor **chỉ trên tập train**, và ghi mọi output ngược lại S3.

#### Processing Job hoàn thành

![SageMaker Processing Job hoàn thành thành công](/fcaj-hcmut-template/images/5-Workshop/W2-01-processing-completed.png)

Trạng thái `Completed` là bằng chứng cho thấy preprocessing đã chạy trên hạ tầng managed, không phải trong một Jupyter kernel ở local. Script chạy bên trong container chính là script sau này sẽ được gắn vào `ProcessingStep` của Pipeline.

#### Báo cáo chất lượng dữ liệu

![Log của Processing Job với số dòng / cột / số lượng split / missing count](/fcaj-hcmut-template/images/5-Workshop/W2-02-processing-log.png)

Các số quan trọng được in ở cuối job:

- Rows: 7.000
- Columns: 22
- Train: 4.900
- Validation: 1.050
- Test: 1.050
- Positive rate: 0,42 (giữ đồng đều qua cả ba split)
- Processed features: 36 (sau khi one-hot encode)
- Missing sau xử lý: 0
- Preprocessor fit scope: `train_only`

Positive rate được giữ qua các split chứng minh stratification đang chạy đúng. Preprocessor chỉ được fit trên `train` là lá chắn chống data leakage từ validation/test vào mô hình.

#### Layout S3 sau xử lý

![Bucket S3 với các split đã xử lý và artifacts tổ chức theo prefix](/fcaj-hcmut-template/images/5-Workshop/W2-03-processed-s3.png)

Sau khi job kết thúc, bucket có layout sau:

```
s3://heart-risk-mlops/
    raw/
        heart_attack_risk.csv
    processed/
        train/train.csv
        validation/validation.csv
        test/test.csv
        raw_split/                ← artifact kiểm tra stratification
    artifacts/
        preprocessor/             ← sklearn ColumnTransformer đã fit
    baseline/                     ← thống kê dùng cho drift detection
    reports/                      ← evaluation metrics, drift reports
    drift/                        ← input/output của custom Processing Job
```

Layout này là thứ mà mọi bước sau (Training, HPO, Evaluation, Pipeline, Drift) phụ thuộc vào. Đổi prefix lúc sau nghĩa là phải chạy lại Processing Job, nên làm đúng từ tuần 2 sẽ tránh phải làm lại ở tuần 3–7.

#### Điều này mở khóa gì

Sau tuần 2, dự án đã có các đầu vào mang tính quyết định mà mọi bước sau cần:

- Ba file CSV ở vị trí cố định.
- Một preprocessor artifact đã fit, bất kỳ request inference nào cũng có thể tái sử dụng.
- Một file baseline statistics để Processing Job drift tuần 7 dùng.

Tác động chi phí: Processing Job này chạy một lần ~10 phút trên `ml.m5.large`, dưới 1 USD.