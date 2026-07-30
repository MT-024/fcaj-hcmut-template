---
title: "Blog 2 - Amazon SageMaker: AI/ML của AWS và Cách tối ưu"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
includeInReport: false
---
{{% notice warning %}}
⚠️ **Note:** For reference only. Do not copy verbatim.
{{% /notice %}}

# BLOG 2
## Amazon SageMaker: AI/ML của AWS và Cách tối ưu để không tốn tiền oan

Chào mọi người,

Trong kỷ nguyên AI hiện nay, không chỉ các tập đoàn lớn mà ngay cả Startups cũng muốn tích hợp Machine Learning vào sản phẩm. Tuy nhiên, việc thiết lập một hạ tầng ML truyền thống với EC2 GPU, cài đặt CUDA, quản lý Jupyter Notebook, và scale inference thường là cơn ác mộng cho các DevOps.

Ra đời để giải quyết bài toán đó, Amazon SageMaker không chỉ là một dịch vụ, mà là một nền tảng (Platform) bao gồm mọi công cụ để bạn xây dựng, huấn luyện và triển khai bất kỳ mô hình ML nào.

Tuy nhiên, "xịn" thì xịn đấy, nhưng nếu không hiểu sâu, hóa đơn AWS cuối tháng sẽ rất "khó đỡ". Dưới đây là kinh nghiệm thực tế của mình để làm chủ SageMaker.

### 1. Hiểu đúng kiến trúc SageMaker (Không chỉ là Notebook!)

Nhiều người mới vào nghề nghĩ SageMaker chỉ là một cái JupyterLab trên Cloud. Thực tế, SageMaker là một hệ sinh thái gồm 3 trụ cột chính:

- **SageMaker Studio:** IDE all-in-one cho Data Scientist để explore dữ liệu và xây dựng model.
- **Training:** Cơ chế quản lý job training, cho phép bạn chạy trên cụm máy chủ mạnh mẽ (P4d, G5) mà không lo về infrastructure.
- **Inference (Hosting):** Triển khai model dưới dạng Endpoint (real-time) hoặc Batch Transform (async).

👉 **Lời khuyết:** Luôn tách bạch SageMaker Studio (dành cho dev/test) và Endpoint (dành cho production). Studio chỉ nên dùng để thử nghiệm với datasets nhỏ. Khi training thật, hãy dùng Jobs.

### 2. "Bí kíp" tối ưu chi phí khi Training Model

Training một mô hình Deep Learning có thể tiêu tốn hàng nghìn USD nếu bạn để quên EC2 Instance chạy 24/7.

💡 **1. Sử dụng "Managed Spot Training"**

Khi bạn tạo một Training Job trên SageMaker, hãy bật chế độ Managed Spot Training.

- **Cơ chế:** SageMaker sẽ tận dụng những máy chủ EC2 dư thừa (Spot Instances) để chạy training với mức giá rẻ hơn tới 70% so với On-Demand.
- **Rủi ro:** Spot có thể bị thu hồi bất kỳ lúc nào. Nhưng SageMaker cực kỳ thông minh: nó sẽ tự động lưu checkpoint và tiếp tục training từ vị trí cuối cùng ngay khi có máy mới. Kiểu "tiết kiệm mà vẫn an toàn".

💡 **2. Warm Start & Hyperparameter Tuning**

Đừng training mô hình từ con số 0 mỗi lần.

- Sử dụng các model đã được pre-train sẵn trên S3 (Ví dụ: ResNet, BERT) và finetune trên dataset của bạn. SageMaker hỗ trợ cơ chế Incremental Training.
- Sử dụng Automatic Model Tuning (Hyperparameter Optimization) nhưng đặt `MaxParallelJobs` và `MaxNumberOfTrainingJobs` ở mức vừa phải. Đừng để nó chạy 500 jobs nếu chưa cần thiết!

💡 **3. Chọn đúng Instance cho Training**

Việc lựa chọn Instance type là yếu tố sống còn:

- **CPU (M5/C5):** Chỉ dùng cho XGBoost, LightGBM, hay thuật toán truyền thống (Linear Learner).
- **GPU (G4dn/G5/P4d):** Dùng cho Deep Learning.
- **G4dn (T4):** Giá rẻ, tốt cho Inference và Training vừa.
- **P4d (A100):** "Siêu máy tính", chỉ dùng khi dataset cực lớn và bạn có budget.

💡 **Mẹo nhỏ:** Đối với training Distributed (phân tán nhiều máy), không phải lúc nào dùng càng nhiều GPU càng nhanh. Đôi khi 2 máy 8 GPU chạy chậm hơn 1 máy 8 GPU do chi phí truyền dữ liệu (Bandwidth bottleneck). Hãy luôn test với job nhỏ trước.

### 3. "Bí kíp" tối ưu chi phí khi Deployment (Inference)

Triển khai model lên Endpoint (Real-time) để phục vụ API là phần phát sinh chi phí lớn nhất vì nó chạy 24/7.

💡 **1. Auto Scaling dựa trên RAM / CPU**

Đừng để Endpoint của bạn luôn chạy với số lượng instance tối đa. Cấu hình Target Tracking Scaling để mở rộng khi CPU > 50% và Scale-in (thu nhỏ) khi traffic thấp (ví dụ về đêm).

Đặc biệt, hãy set `MinInstanceCount = 0` cho các môi trường Staging để tự động tắt khi không dùng (Serverless inference).

💡 **2. Serverless Inference (SageMaker Serverless)**

Mới ra mắt gần đây, đây là "người anh em" của Lambda dành cho ML.

- Bạn không cần quản lý EC2 nữa. SageMaker tự scale từ 0 lên tối đa (Max concurrency).
- **Phù hợp:** Các API có tần suất thấp, không yêu cầu độ trễ (latency) cực thấp.
- **Không phù hợp:** Hệ thống yêu cầu traffic đều đặn > 100 requests/phút (vì lúc này EC2 sẽ rẻ hơn).

💡 **3. Tối ưu Model (Model Optimization)**

Đây là kỹ thuật để giảm size file và tăng tốc độ inference mà mình thấy ít người làm:

- **Quantization:** Chuyển weight từ FP32 xuống INT8. Các model như PyTorch hay TensorRT đều hỗ trợ. Việc này giúp giảm dung lượng xuống 4 lần và tăng tốc inference gấp đôi mà độ chính xác hầu như không đổi.
- **SageMaker Neo:** Nếu bạn deploy lên các thiết bị edge (tiết kiệm) hoặc muốn tối ưu cho CPU, hãy dùng SageMaker Neo để biên dịch model thành mã lệnh cực nhanh.

### 4. Đừng quên MLOps: CI/CD cho AI

SageMaker tích hợp cực tốt với SageMaker Pipelines (dựa trên Kubeflow). Đây là cách để bạn biến ML thành một ứng dụng phần mềm thực thụ:

1. Code commit lên Git (CodeCommit/GitHub).
2. Pipeline tự động trigger để chạy Unit Test trên code, train model trên dữ liệu mới.
3. Đánh giá model (Model Registry) - Nếu độ chính xác (Accuracy) cao hơn version cũ, tự động deploy lên Staging.
4. Kiểm tra A/B Testing với Production Variants (Canary deployment) trước khi đẩy hết traffic sang version mới.

### 5. Tổng kết: SageMaker có đáng tiền?

Câu trả lời là CÓ, nếu bạn biết cách sử dụng. Việc tự build cluster GPU trên EC2 và cài đặt Kubernetes để scale sẽ tốn kém và cực kỳ mất thời gian. SageMaker giải phóng bạn khỏi tất cả tầng "hạ tầng khô khan" đó.

**Lời khuyên cuối cùng:**

Đừng xem SageMaker như một "chiếc máy tính xách tay". Hãy xem nó như một "Nhà máy sản xuất AI". Sử dụng Spot cho Training, Serverless cho Staging, và Auto Scaling + Neo cho Production. Khi đó, bạn sẽ có một pipeline AI mạnh mẽ mà chi phí vô cùng hợp lý.

---

**Link FB post:** https://www.facebook.com/groups/awsstudygroupfcj/posts/2227364341361859/