---
title: "Blog 1 - AWS Lambda: Chiến lược \"xài đúng\" và \"chạy nhanh\""
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
includeInReport: false
---

# BLOG 1
## AWS Lambda : Chiến lược "xài đúng" và "chạy nhanh" để tối ưu chi phí

Chào mọi người,

Trong thế giới Cloud hiện đại, Serverless đã thoát khỏi vai trò của một "buzzword" để trở thành tiêu chuẩn vàng cho các kiến trúc linh hoạt. Với AWS Lambda, chúng ta không còn phải đau đầu với việc scale EC2 hay bảo trì OS, mà dành toàn bộ tâm trí để viết logic nghiệp vụ.

Tuy nhiên, không phải cứ "đẩy lên Lambda" là xong. Chạy Lambda sao cho nhanh, rẻ và đúng là một bài toán cần chiến lược. Dưới đây là bài học mình rút ra khi làm project và lab.

### 1. Phân định "Battlefield": Khi nào chọn Lambda, khi nào né?

AWS Lambda cực mạnh, nhưng không phải là "cây đũa thần". Có những bài toán chọn Lambda là "thiên đường", nhưng cũng có bài toán chọn Lambda là "địa ngục".

✅ **Những bài toán "thiên đường" (Should Use):**

- **Xử lý không đồng bộ (Async/Event-driven):** Là vua của mọi use-case. Ví dụ: Khi có file CSV upload lên S3, Lambda sẽ tự động đọc, parse và insert vào DynamoDB. Không có request chờ đợi, Lambda chạy ngầm và thoát.
- **API có tần suất thấp hoặc đột biến (Spiky traffic):** Dành cho Web/Mobile App Backend qua API Gateway. Lambda scale lên 0 khi không có traffic và bung hàng ngàn instance trong vài giây khi có đợt sale lớn (Black Friday).
- **Tác vụ nền theo lịch (Cron jobs):** Thay vì để một con EC2 chạy 24/7 chỉ để mỗi sáng chạy báo cáo, bạn dùng EventBridge (CloudWatch Events) kích hoạt Lambda. Chi phí giảm tới 90%.

❌ **Những bài toán "địa ngục" (Should Avoid):**

- **Tác vụ chạy dài (Heavy Compute):** Lambda có giới hạn 15 phút. Nếu bạn đang xử lý video dung lượng lớn hay training model AI, hãy dùng Fargate hoặc Batch.
- **Yêu cầu độ trễ cực thấp (Ultra-low latency < 50ms):** Nếu application của bạn là game hay giao dịch tài chính yêu cầu latency tuyệt đối, Lambda lạnh (Cold Start) có thể gây hại.
- **Trạng thái (Stateful):** Lambda là "Stateless". Đừng cố lưu file hay session trong /tmp của Lambda nếu không muốn mất dữ liệu khi instance bị kill.

### 2. Chìa khóa "Vàng" để tối ưu hiệu năng và chi phí

Để đạt hiệu năng tối đa, chúng ta không thể chỉ xài Lambda kiểu cơ bản. Cần một tư duy tối ưu xuyên suốt:

💡 **1. "Execution Environment" & Static Initialization**

Đây là nguyên tắc quan trọng nhất. Lambda dùng cơ chế Container reuse (các thùng chứa được giữ lại trong vài giờ để xử lý tiếp request).

👉 **Action:** Khởi tạo tất cả SDK, Database Connection, và HTTP Client (Axios/Requests) ở bên ngoài hàm handler.

```python
# Sai
def handler(event, context):
    client = boto3.client('s3')  # Khởi tạo mỗi khi gọi -> Tốn thời gian

# Đúng
client = boto3.client('s3')  # Global scope
def handler(event, context):
    client.get_object(...)  # Tái sử dụng ngay lập tức
```

💡 **2. "Lambda Power Tuning" – Tăng RAM = Rẻ hơn**

Điều này nghe phản trực giác, nhưng thực tế là Tăng RAM (Memory) sẽ tăng vCPU tuyến tính. Nếu bạn tăng từ 512MB lên 1769MB, thời gian xử lý có thể giảm đi 50%. Vì Lambda tính phí dựa trên (Thời gian x Giây) x (RAM), đôi khi chạy nhanh hơn gấp đôi với RAM gấp đôi đồng nghĩa với tổng chi phí không đổi hoặc thấp hơn, trong khi hiệu năng thì vượt trội.

💡 **Mẹo:** Dùng tool mã nguồn mở AWS Lambda Power Tuning để chạy thử nghiệm và tìm ra mức RAM tối ưu nhất cho function của bạn (thường nằm ở mức 1024MB hoặc 1769MB).

💡 **3. Giảm thiểu Cold Start (Khởi tạo nguội)**

Cold Start là khoảng thời gian Lambda phải tải runtime (Node.js/Python/Java) và code của bạn từ S3 lên.

- Nếu dùng Java/C# (.NET): Nhẹ đời nhất là dùng GraalVM hoặc Native Compilation để giảm thời gian khởi tạo từ vài giây xuống vài trăm mili giây.
- Provisioned Concurrency: Nếu bạn cần độ trễ thấp tuyệt đối cho API critical, hãy bật Provisioned Concurrency (giữ sẵn instance). Lưu ý: tính phí cho instance chạy 24/7, nên chỉ áp dụng cho hàm có tần suất cao.

💡 **4. EFS vs Lambda Layers vs Package size**

- **Package size:** Luôn luôn zipp code, loại bỏ thư mục tests và `__pycache__`.
- **Lambda Layers:** Dùng để tách các thư viện nặng (Pandas, Numpy, AWS SDK) ra khỏi code chính. Nhờ vậy, code của bạn siêu nhẹ, deployment nhanh, và bản thân Layer đã được cache sẵn ở edge.

💡 **5. Kết nối DB đỉnh cao: RDS Proxy**

Đây là cứu tinh cho các hệ thống dùng MySQL/PostgreSQL truyền thống. Lambda có thể scale lên hàng nghìn instance cùng lúc, kéo theo hàng nghìn kết nối tới Database. Điều này sẽ "đốt cháy" DB của bạn.

👉 **Giải pháp:** Luôn đặt RDS Proxy giữa Lambda và RDS. Proxy sẽ gom các kết nối lẻ thành một pool thông minh, bảo vệ Database khỏi bị quá tải (Connection Exhaustion).

### 3. Monitoring và Debugging "Xịn"

Đừng để Lambda của bạn chạy như một "chiếc hộp đen". Hãy tận dụng:

- **AWS X-Ray:** Bật X-Ray để trace request của bạn. Nó giúp bạn biết chính xác 50% thời gian xử lý bị tiêu tốn ở đâu (Do SDK gọi S3 lâu? Hay API bên thứ 3 chậm?).
- **Custom Metrics (EMF):** Dùng nhúng metric riêng vào CloudWatch để vẽ biểu đồ thời gian chạy (Duration) theo từng version hàm.

### 4. Tổng kết: Mindset của một Serverless Engineer

Làm chủ AWS Lambda không chỉ là việc viết function, mà là một tư duy kiến trúc:

- **Thiết kế hướng sự kiện (Event-Driven):** Hãy tách bạch các service. Lambda A ghi file vào S3 -> trigger Lambda B.
- **Fault Tolerance:** Luôn thiết lập DLQ (Dead Letter Queue) qua SQS để bắt các lỗi không xử lý được, tránh mất data.
- **Giới hạn (Limits):** Luôn nhớ các giới hạn mềm (Soft limits) như 1000 concurrent executions mặc định. Nếu dự án của bạn lớn hơn, hãy xin AWS tăng limit từ trước.

---

**Link FB post:** https://www.facebook.com/groups/awsstudygroupfcj/posts/2227143931383900