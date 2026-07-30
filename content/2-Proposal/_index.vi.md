---
title: "Bản đề xuất"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
includeInReport: false
---

# Dự đoán nguy cơ đau tim trên AWS SageMaker
## Pipeline MLOps end-to-end cho bài toán phân loại nhị phân, ngân sách 200 USD

### 1. Tóm tắt điều hành
Đồ án Heart-Attack-Risk Prediction là một hệ MLOps hoàn chỉnh, đưa một bộ dữ liệu lâm sàng dạng 7.000 dòng từ CSV thô đến một REST API đã triển khai và có giám sát cho bài toán phân loại nhị phân. Toàn bộ hệ thống dựng trên AWS SageMaker và các managed service liên quan, chạy trong một region duy nhất (`ap-southeast-1`), và bị ràng buộc bởi budget cứng 200 USD, không dùng GPU, không dùng NAT Gateway.

Pipeline được biểu diễn dưới dạng code: tiền xử lý dữ liệu, huấn luyện mô hình, đánh giá, đăng ký có điều kiện và phê duyệt thủ công được nối với nhau qua SageMaker Pipelines. Endpoint triển khai được wrap bởi một Lambda function và API Gateway cung cấp hai route là `GET /health` và `POST /predict`. Data Capture chạy với sampling rate 100 %, và một custom Processing Job sẽ đẩy metric drift lên CloudWatch cùng một alarm trong namespace `Custom/HeartRisk`.

Mô hình triển khai được chọn là **Logistic Regression** (test ROC-AUC 0,885515; F1 0,768903; recall 0,818594), vượt XGBoost cả trước và sau HPO trên metric chính. Lưu ý: kết quả dự đoán là output của mô hình, không phải chẩn đoán y khoa.

### 2. Tuyên bố vấn đề
#### Vấn đề hiện tại
Phần lớn đồ án ML của sinh viên dừng ở mức "notebook train được một mô hình". Production deployment, versioning, drift monitoring và kỷ luật chi phí thường nằm ngoài phạm vi. Đồ án capstone của FCAJ yêu cầu đi đủ vòng đời trên AWS, nhưng với cùng ràng buộc mà một team thật phải đối mặt: ngân sách chặt, một region, không GPU, và không có " lát sẽ sửa sau" về chi phí.

Tập dữ liệu dự đoán nguy cơ đau tim (7.000 dòng, 22 cột, target là `heart_attack_risk`) đủ nhỏ để nằm trong cap 200 USD nhưng đủ giàu để các quyết định chất lượng dữ liệu — class balance, xử lý missing value, validate schema — thực sự có ý nghĩa. Deliverable không phải một notebook; đó là pipeline có thể chạy lại end-to-end chỉ bằng một lệnh `pipeline.start()`.

#### Giải pháp
Hệ thống dùng Amazon S3 để lưu trữ dữ liệu và artifacts, SageMaker Processing cho tiền xử lý, SageMaker Training và HPO cho các mô hình ứng viên, SageMaker Model Registry với phê duyệt thủ công để quản lý phiên bản, và một real-time endpoint SageMaker cho inference. Lambda + API Gateway đưa mô hình ra thành REST API. Một custom Processing Job — dùng làm phương án fallback khi metric của Model Monitor chính thức chưa xuất hiện đúng lúc — đọc output Data Capture từ S3, tính drift theo feature, và publish `DriftDetected` cùng `DataQualityViolationCount` lên CloudWatch trong `Custom/HeartRisk`. Một CloudWatch alarm sẽ chuyển sang ALARM khi phát hiện drift.

#### Lợi ích và ROI
- **Tái lập**: pipeline-as-code nghĩa là một `pipeline.start()` trên môi trường mới vẫn reproduce được preprocessing, training, evaluation và (có điều kiện) registration.
- **Versioning & governance**: Model Registry + manual approval gate giữ con người trong vòng kiểm soát trước khi bất kỳ mô hình nào tới được API.
- **Khả năng quan sát**: Data Capture + custom drift Processing Job + CloudWatch alarm cho ra một chuỗi cảnh báo chạy được trong budget 200 USD, không phải trả tiền cho SageMaker Model Monitor.
- **Kỷ luật chi phí**: Spot Training, instance type tương thích serverless, lifecycle rule trên S3, một endpoint instance giới hạn cho cửa sổ demo, và script `cleanup.py` dọn tài nguyên chạy liên tục.
- **Giá trị đào tạo**: các pattern tương tự (pipeline as code, quality gate, drift fallback, IAM scoping) chuyển trực tiếp sang vai trò MLOps production.

### 3. Kiến trúc giải pháp
Sơ đồ dưới đây phản ánh đúng luồng đã triển khai (S3 → Processing → Train/HPO → Registry → Endpoint → API, với đường drift chạy song song và một pipeline điều phối):

![Kiến trúc hệ thống Heart-Attack-Risk Prediction](/images/2-Proposal/aws-flow.jpg)

#### Dịch vụ AWS sử dụng
- **Amazon S3** — lưu raw data, processed splits, baseline statistics, drift report và pipeline artifacts.
- **Amazon SageMaker Processing** — chạy preprocessing và custom drift-detection job.
- **Amazon SageMaker Training** — huấn luyện Logistic Regression và XGBoost; hỗ trợ Spot Training.
- **Amazon SageMaker HPO** — chạy tuning XGBoost với số trial giới hạn (3 trials, `max_parallel_jobs=1`) để kiểm soát chi phí.
- **Amazon SageMaker Model Registry** — quản lý phiên bản mô hình ứng viên; pipeline chỉ đăng ký các mô hình vượt quality gate.
- **Amazon SageMaker Real-Time Endpoint** — một instance `ml.m5.large` duy nhất với Data Capture bật ở 100 % cho cả input và output.
- **AWS Lambda** — lớp mỏng validate payload và gọi SageMaker endpoint; IAM scope least-privilege (chỉ `sagemaker:InvokeEndpoint` trên endpoint của dự án).
- **Amazon API Gateway** — cung cấp `GET /health` và `POST /predict` (AWS_PROXY integration).
- **Amazon CloudWatch** — namespace `Custom/HeartRisk` cho `DriftDetected` và `DataQualityViolationCount`, kèm alarm chuyển sang ALARM khi có drift.
- **AWS Budgets + SNS** — cost guardrail dựng trước khi bất kỳ resource tính phí nào được tạo.

#### Thiết kế thành phần
- **Tầng dữ liệu**: bucket S3 chứa `raw/`, `processed/{train,validation,test}/`, `artifacts/preprocessor/`, `baseline/`, `reports/` và `drift/`.
- **Tiền xử lý**: SageMaker Processing Job kiểm tra schema, impute/encode feature, chia 70/15/15 có stratification, ghi preprocessor artifacts. Preprocessor chỉ được fit trên tập train.
- **Huấn luyện**: Logistic Regression và XGBoost được train; HPO tinh chỉnh XGBoost với `validation:auc` làm objective metric.
- **Đánh giá**: một managed evaluation step tính ROC-AUC, F1, recall, precision, accuracy và confusion matrix trên tập test.
- **Quality gate**: `ConditionStep` kiểm tra `roc_auc >= 0,84 AND f1 >= 0,70 AND recall >= 0,65`; mô hình đạt đi tới `RegisterModel`, mô hình không đạt đi tới `FailStep`.
- **Registry & approval**: các Model Package version rơi vào `PendingManualApproval`. Một người phải duyệt trước khi mô hình được triển khai.
- **Inference path**: API Gateway → Lambda (validate → invoke) → SageMaker endpoint → response (gồm `prediction`, `risk_probability`, `threshold`, `model_type`, disclaimer).
- **Drift path**: Data Capture (S3) → EventBridge rule theo giờ → custom Processing Job (standardized mean shift > 0,5 cho numeric, total variation distance > 0,20 cho categorical) → CloudWatch metrics → CloudWatch alarm.

### 4. Triển khai kỹ thuật
#### Các giai đoạn triển khai
Kỳ thực tập FCAJ 8 tuần ánh xạ gần đúng một phase mỗi tuần:
- **Tuần 1 — Onboarding + brief**: chốt budget cap (200 USD), region (`ap-southeast-1`), ràng buộc không GPU và không NAT; dựng AWS account, CLI và Budgets alarm.
- **Tuần 2 — Dữ liệu + S3/IAM**: định nghĩa layout dữ liệu trên S3, dựng Processing Job cho preprocessing, validate schema và splits.
- **Tuần 3 — Baseline training**: train Logistic Regression và XGBoost, so sánh metric validation.
- **Tuần 4 — HPO**: tinh chỉnh XGBoost với search space nhỏ, kiểm soát chi phí (3 trials, `max_parallel_jobs=1`).
- **Tuần 5 — Model Registry + Endpoint**: nối evaluation, quality gate, manual approval và real-time endpoint đầu tiên.
- **Tuần 6 — API layer**: Lambda + API Gateway, validate request, xử lý HTTP 200/400/502, bật Data Capture.
- **Tuần 7 — Drift detection**: custom Processing Job + EventBridge + CloudWatch metrics + alarm.
- **Tuần 8 — Pipeline + cleanup**: pipeline đầy đủ `Processing → Training → Evaluation → Condition → Register/Fail`; `cleanup.py` dọn tài nguyên chạy liên tục.

#### Yêu cầu kỹ thuật
- **Dữ liệu**: 7.000 dòng, 22 cột, 20 đặc trưng đầu vào sau khi bỏ `patient_id` và target; chia stratified 70/15/15 (4.900 / 1.050 / 1.050).
- **Mô hình**: Logistic Regression (được chọn) và XGBoost (kèm HPO). Logistic Regression thắng theo validation ROC-AUC: 0,863949 so với XGBoost mặc định 0,854283 và XGBoost-sau-HPO 0,860982.
- **Quality gate**: ROC-AUC ≥ 0,84, F1 ≥ 0,70, recall ≥ 0,65.
- **Rule phát hiện drift**: numeric dùng standardized mean shift > 0,5; categorical dùng total variation distance > 0,20. Các rule phục vụ minh họa PoC, không phải chuẩn lâm sàng.
- **IAM**: SageMaker execution role scope trong bucket dự án + endpoint dự án; Lambda execution role chỉ có `sagemaker:InvokeEndpoint` trên `heart-risk-endpoint`.
- **Kiểm soát chi phí**: Spot Training nếu hỗ trợ, một endpoint instance, lifecycle rule trên logs/artifacts, script `cleanup.py` lọc theo tag `Project=heart-risk-mlops`, AWS Budget alarm.

### 5. Lộ trình & Mốc triển khai
**Lộ trình dự án**
- **Trước thực tập**: đọc tài liệu FCAJ, chốt brief capstone, lên kế hoạch dữ liệu và chi phí.
- **Thực tập (8 tuần, 01/06/2026 → 24/07/2026)**:
  - Tuần 1: Onboarding + đọc brief.
  - Tuần 2: Preprocessing trên S3 + IAM.
  - Tuần 3: So sánh XGBoost vs Logistic Regression.
  - Tuần 4: HPO trên XGBoost.
  - Tuần 5: Model Registry + endpoint.
  - Tuần 6: Lambda + API Gateway + Data Capture.
  - Tuần 7: Phát hiện drift + CloudWatch alarm.
  - Tuần 8: SageMaker Pipeline + cleanup.
- **Sau thực tập**: lưu trữ artifacts, viết báo cáo, thuyết trình demo.

### 6. Ước tính ngân sách
**Chi phí hạ tầng (ước tính, cap 200 USD, một region `ap-southeast-1`)**
- Amazon S3: lưu raw/processed/artifacts + lifecycle rule trên logs → vài USD trong suốt 8 tuần.
- SageMaker Processing: 2 job (preprocessing + drift), thời gian chạy ngắn → vài USD.
- SageMaker Training + HPO: 3 HPO trials + 2 baseline training job, dùng Spot nếu được → ước tính dưới 30 USD.
- SageMaker Endpoint: một instance `ml.m5.large` duy nhất giới hạn trong các cửa sổ demo (vài ngày tổng cộng) → ước tính dưới 60 USD.
- Lambda + API Gateway: request volume vài trăm → ước tính dưới 5 USD.
- CloudWatch metrics + alarm: namespace `Custom/HeartRisk`, 2 custom metric → ước tính dưới 5 USD.
- Data transfer trong `ap-southeast-1`: không đáng kể.

Hạng mục đắt nhất là endpoint, vì vậy nó được giới hạn trong các cửa sổ demo và dọn bởi `cleanup.py`. Không dùng GPU instance, không dùng NAT Gateway; Spot Training được dùng ở những nơi hỗ trợ.

> Lưu ý: AWS Billing và Cost Explorer có độ trễ báo cáo. Hóa đơn cuối kỳ thực tế được đối chiếu qua lịch sử AWS Budget alarm thay vì một snapshot dashboard duy nhất.

### 7. Đánh giá rủi ro
#### Ma trận rủi ro
- **Endpoint idle cost**: ảnh hưởng trung bình, xác suất trung bình → giảm thiểu bằng cách giới hạn endpoint trong các cửa sổ demo và script `cleanup.py`.
- **Drift báo động giả (hoặc bỏ sót drift)**: ảnh hưởng trung bình, xác suất trung bình → giảm thiểu bằng lịch chạy theo giờ cố định và rule được định nghĩa rõ; thừa nhận đây là rule PoC, không phải chuẩn lâm sàng.
- **Vượt ngân sách khi chạy HPO**: ảnh hưởng trung bình, xác suất thấp → giảm thiểu bằng giới hạn 3 trials và `max_parallel_jobs=1`.
- **Pipeline fail chặn tiến độ**: ảnh hưởng trung bình, xác suất thấp → giảm thiểu bằng manual approval gate và một lần chạy failure có chủ đích được tài liệu hóa.
- **IAM cấu hình sai**: ảnh hưởng cao, xác suất thấp → giảm thiểu bằng hai role tách biệt (SageMaker vs Lambda) và policy least-privilege.

#### Chiến lược giảm thiểu
- **Chi phí**: AWS Budgets alarm ở mốc 50 / 80 / 100 %; mọi resource tính phí được gắn tag `Project=heart-risk-mlops`; `cleanup.py` là một phần của deliverable.
- **Drift**: threshold được tài liệu hóa và custom Processing Job độc lập với khả dụng của managed Model Monitor.
- **Bảo mật**: Lambda role không đọc được S3 và không gọi được `sagemaker:CreateTrainingJob`; SageMaker role không invoke được endpoint tùy ý.

#### Kế hoạch dự phòng
- Nếu metric của Model Monitor xuất hiện trễ, có thể swap custom Processing Job sang managed schedule mà không đổi hợp đồng alarm.
- Nếu chi phí endpoint trở thành mối lo, chuyển sang Serverless Inference cho các cửa sổ traffic thấp.

### 8. Kết quả kỳ vọng
#### Cải tiến kỹ thuật
- Một pipeline MLOps end-to-end, có thể tái lập, chạy bằng một lệnh `pipeline.start()`.
- Một mô hình có versioning trong SageMaker Model Registry với manual approval gate.
- Một REST API đã triển khai (`GET /health`, `POST /predict`) chạy trên Lambda + API Gateway + SageMaker endpoint.
- Một chuỗi cảnh báo drift chạy được: Data Capture → custom Processing Job → CloudWatch metrics → CloudWatch alarm.
- Metric trên tập test: ROC-AUC 0,885515; F1 0,768903; recall 0,818594 — vượt quality gate của dự án.

#### Giá trị dài hạn
- Một template có thể tái sử dụng cho các khóa FCAJ sau về SageMaker MLOps có kiểm soát chi phí.
- Một sản phẩm portfolio cá nhân thể hiện pipeline-as-code, thiết kế drift fallback và IAM scoping trong một budget thật.
- Nền tảng cho lộ trình tiếp theo về production hardening (multi-AZ failover, IaC refactor, canary deployment).

> Lưu ý: kết quả dự đoán từ hệ thống là output mô hình phục vụ mục đích học tập. Đây không phải chẩn đoán y khoa và không được dùng thay thế tư vấn của bác sĩ chuyên môn.
