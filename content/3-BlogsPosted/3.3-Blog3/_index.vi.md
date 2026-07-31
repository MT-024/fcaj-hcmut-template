---
title: "Blog 3 - Chạy SageMaker MLOps với 200 USD: 13 quyết định để không cháy budget"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
includeInReport: false
---


# BLOG 3
## Chạy SageMaker MLOps với 200 USD: 13 quyết định để không cháy budget

**Disclaimer:** Bài viết chia sẻ kinh nghiệm từ một dự án capstone sinh viên trên AWS SageMaker (binary classifier dự đoán nguy cơ đau tim – mục đích giáo dục). Mọi con số chi phí là **ước tính cho môi trường học tập** ở thời điểm mình làm, có thể đã thay đổi. Bài viết **không phải** tư vấn tài chính hay hướng dẫn tối ưu chi phí AWS chính thức. Vui lòng tự kiểm tra AWS Pricing Calculator trước khi áp dụng cho dự án thật.

---

### MỞ ĐẦU: 200 USD NGHE NHƯ NHIỀU — CHO ĐẾN KHI BẠN TÍNH

Khi đọc brief lần đầu, con số **200 USD** trông như một ngân sách dư dả. Rồi mình ngồi làm một phép tính đơn giản:

- 8 tuần × 2 buổi/tuần × 4 giờ/buổi = **64 giờ** thao tác với AWS
- Một endpoint `ml.t2.medium` chạy 24/7: ~**35 USD/tháng**
- Một HPO job train 6 trials × ~20 phút/trial trên `ml.m5.large`: ~**3 USD/lần**
- S3 storage qua 8 tuần cộng dồn data, log, artifact: ~**5–8 USD**
- CloudWatch, Lambda, API Gateway, Data Capture: ~**5–10 USD** nhỏ giọt

Cộng lại nghe có vẻ "vẫn dư". Nhưng chi phí AWS **không tăng tuyến tính theo effort** — nó tăng theo **sự bất cẩn**.

- Một endpoint để chạy cả tuần thay vì tắt sau demo.
- Một S3 bucket không có lifecycle.
- Một job HPO chạy 30 trials thay vì 6.

Mỗi sai lầm nhỏ cộng dồn thành một cú bill bất ngờ.

Mình đã học được rằng:

> **"Trên cloud, 'tắt sau khi dùng' là kỹ năng quan trọng hơn 'chạy đúng hay sai'."**

Đây là 13 quyết định đã giúp mình cán đích với budget cap 200 USD mà không phải gồng bill.

---

### QUYẾT ĐỊNH 1–4: COMPUTE

**Quyết định #1: Chọn `ml.t3.medium` làm instance mặc định**

Thay vì dùng `ml.m5.large`, mình thử train XGBoost với ~7.000 dòng dữ liệu trên `ml.t3.medium`.

Kết quả:
- Training ~4 phút
- AUC đạt 0.86

→ Với dataset nhỏ (<100K dòng), `t3.medium` hoàn toàn đủ dùng.

**Bài học:** Đừng nâng cấp instance chỉ vì "cho chắc". Hãy benchmark trước.

**Cost:**
- `ml.t3.medium`: ~$0.05/giờ
- `ml.m5.large`: ~$0.134/giờ (gấp ~2.7 lần)

**Quyết định #2: KHÔNG dùng GPU**

Project không cần GPU. Một notebook `ml.g4dn.xlarge` chỉ cần để idle cũng tiêu tốn ~$0.736/giờ.

**Bài học:** GPU chỉ đáng tiền khi model hoặc dataset đủ lớn.

**Quyết định #3: Chỉ dùng một Region**

Mình chọn `ap-southeast-1` (Singapore). Không phải vì hiệu năng, mà để tránh phải nhân đôi IAM Role, S3 Bucket, Security Group...

**Bài học:** Project sinh viên không cần multi-region.

**Quyết định #4: Tắt Endpoint ngay sau Demo**

Đây là quyết định tiết kiệm tiền nhất.

- Endpoint chạy 24/7 suốt 8 tuần: ~35 USD
- Chỉ bật lúc demo: ~5 USD
- **Tiết kiệm ~30 USD** (15% tổng budget).

**Bài học:** Endpoint tồn tại là đã tính tiền, kể cả không có request.

---

### QUYẾT ĐỊNH 5–8: DATA & STORAGE

**Quyết định #5: Dùng S3 Lifecycle Rule**

Log, artifact và model sẽ tự xóa sau 30 ngày.

**Bài học:** Lifecycle gần như miễn phí nhưng tiết kiệm được khá nhiều storage. Tiết kiệm ~3–5 USD.

**Quyết định #6: Chỉ preprocess dataset một lần**

Thay vì mỗi lần training lại preprocess dữ liệu, mình lưu dataset đã xử lý vào thư mục `processed/`. Các lần sau chỉ đọc lại.

**Tiết kiệm:** ~1–2 USD.

**Quyết định #7: Không retrain vì những cải thiện rất nhỏ**

AUC từ 0.850 lên 0.855 chưa chắc mang lại giá trị thực tế.

**Bài học:** "Đủ tốt" cũng là một quyết định kỹ thuật. Tiết kiệm ~3–5 USD.

**Quyết định #8: Tag toàn bộ Resource**

Mọi resource đều được gắn:
- `Project`
- `Owner`
- `Environment`
- `AutoDelete=true`

Sau khi project kết thúc chỉ cần chạy cleanup script.

**Bài học:** Không tag thì rất dễ quên resource và bị tính tiền.

---

### QUYẾT ĐỊNH 9–11: PIPELINE & TRAINING

**Quyết định #9:** Giới hạn HPO: `max_parallel_jobs = 1`, `max_jobs = 6`. Toàn bộ HPO chỉ ~0.6 USD.

**Quyết định #10:** Chạy cleanup script mỗi tuần. Script xóa endpoint cũ, dọn CloudWatch Logs, xóa resource không cần thiết.

**Bài học:** CloudWatch không tự dọn log.

**Quyết định #11:** Không dùng Notebook Instance để train. Mình chỉ dùng SageMaker Training Job. Training xong là instance tự hủy.

**Bài học:** Notebook và Training Job có cách tính phí hoàn toàn khác nhau.

---

### QUYẾT ĐỊNH 12–13: IAM & NETWORK

**Quyết định #12:** Không bao giờ dùng `AdministratorAccess`. IAM chỉ cấp đúng quyền cần thiết.

**Bài học:** Least Privilege vừa an toàn vừa tránh những sai sót tốn kèm.

**Quyết định #13:** Không dùng NAT Gateway. NAT Gateway có thể tiêu tốn ~30 USD/tháng.

Thay thế bằng:
- Gateway Endpoint
- Interface Endpoint
- Public Internet (khi phù hợp)

**Bài học:** Đừng bật NAT Gateway nếu không thật sự cần.

---

### TỔNG KẾT CHI PHÍ (ƯỚC TÍNH)

| Hạng mục | Không tối ưu | Có tối ưu |
| -------- | ------------ | --------- |
| Endpoint | 35 USD | 5 USD |
| S3 | 10 USD | 5 USD |
| HPO | 3 USD | 0.6 USD |
| Training | 10 USD | 3 USD |
| NAT Gateway | 30 USD | 0 USD |
| CloudWatch | 5 USD | 1 USD |
| Misc | 10 USD | 2 USD |
| **Tổng** | **~113 USD** | **~26.6 USD** |

Thực tế project của mình dao động khoảng **80–110 USD** cho toàn bộ 8 tuần, vẫn còn khá nhiều buffer dưới mức 200 USD.

---

### CHECKLIST TRƯỚC KHI TẠO RESOURCE

- [ ] Resource này thật sự cần không?
- [ ] Có thể dùng `t3.medium` thay vì `m5` không?
- [ ] Resource có tự xóa không?
- [ ] Đã gắn tag `Project` và `AutoDelete` chưa?
- [ ] IAM có đúng phạm vi chưa?
- [ ] Endpoint hoặc Notebook đã có lịch tắt chưa?
- [ ] S3 đã có Lifecycle Rule chưa?
- [ ] Nếu quên tắt thì một tuần sẽ tốn bao nhiêu USD?

---

### KẾT

Trước đây mình nghĩ dùng AWS đơn giản là build xong rồi dọn. Sau project này mình nhận ra:

> **Bạn phải nghĩ đến việc dọn ngay từ lúc thiết kế.**

Budget cap không phải là giới hạn. Nó buộc bạn thiết kế pipeline tốt hơn:
- Có lifecycle
- Có tagging
- Có cleanup
- Có IAM rõ ràng

Những thứ này vẫn hữu ích ngay cả khi ngân sách tăng gấp 10 lần.

Nếu bạn đang làm project AWS với budget giới hạn, **đừng xem đó là bất lợi**. Hãy xem đó là cơ hội để học cách thiết kế hệ thống đúng ngay từ đầu.

![Your profile picture](/fcaj-hcmut-template/images/blog-3.jpg)
