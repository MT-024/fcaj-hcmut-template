---
title: "Tuần 6"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Thứ
  - Công việc
  - Ngày hoàn thành
reportHeadings:
  - Mục tiêu tuần 6
  - Công việc cần làm trong tuần
  - Thành quả tuần 6
---
{{% notice warning %}}
⚠️ **Lưu ý:** Chỉ mang tính tham khảo.
{{% /notice %}}


### Mục tiêu tuần 6:

* Expose Endpoint qua public HTTP API.
* Thêm disclaimer vào mọi response (yêu cầu brief).
* Hiện thực lifecycle automation cho endpoint (bật/tắt theo lịch).


### Công việc cần làm trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| --- | --------- | ------------ | --------------- |
| 2   | - Tạo Lambda function `heart-risk-invoke` với IAM role giới hạn `sagemaker:InvokeEndpoint` chỉ trên endpoint ARN | 09/09/2025 | 09/09/2025 |
| 3   | - Viết Lambda handler: parse request, gọi `sm.invoke_endpoint()`, nối disclaimer vào response <br> - Verify response format: `{"prediction": 0/1, "probability": 0..1, "disclaimer": "..."}` | 10/09/2025 | 11/09/2025 |
| 4   | - Tạo API Gateway REST API `heart-risk-api` <br> - Kết nối Lambda qua AWS_PROXY integration | 11/09/2025 | 12/09/2025 |
| 5   | - Test API bằng curl/Postman: gửi patient features → verify JSON response có disclaimer | 12/09/2025 | 13/09/2025 |
| 6   | - Hiện thực endpoint scheduler (bật 19h thứ 6 demo, tắt 23h cùng ngày) <br> - Test 1 cycle | 13/09/2025 | 13/09/2025 |


### Thành quả tuần 6:

* Lambda + API Gateway public API chạy được.
* Mọi response đều có disclaimer `"Educational demonstration only; not a medical diagnosis."`
* Endpoint scheduler tiết kiệm ~30 USD/tháng so với chạy 24/7.

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Tuần 6 - API + Lambda                         -->
<!-- Cần điền:                                                       -->
<!--   - API Gateway URL                                             -->
<!--   - Lambda ARN                                                  -->
<!--   - Sample request/response                                     -->
<!--   - Cost tuần 6                                                 -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->


### Tài liệu tham khảo:

* https://docs.aws.amazon.com/lambda/latest/dg/with-sagemaker.html