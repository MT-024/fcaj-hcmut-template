---
title: "FCAJ x AABW Hackathon"
date: 2026-07-25
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
includeInReport: false
---

# Bài thu hoạch: FCAJ x AABW Hackathon (Agentic AI Build Week)

### Thông tin sự kiện

| | |
| --- | --- |
| **Tên sự kiện** | FCAJ x AABW Hackathon — Agentic AI Build Week |
| **Thời gian** | ~25/07/2026 (xác nhận lại với BTC nếu sai) |
| **Địa điểm** |Tầng 26, tòa nhà Bitexco, số 02 đường Hải Triều, phường Sài Gòn, thành phố Hồ Chí Minh <!-- TODO: địa điểm thực tế (vd: TP.HCM hoặc Hà Nội) --> |
| **Vai trò** | Người tham dự |
| **Định dạng** | 4 nhóm pitch sản phẩm AI dùng AWS services, mỗi nhóm trình bày sản phẩm end-to-end |

### Mục đích của sự kiện

- Kết hợp FCAJ (cộng đồng AWS tại Việt Nam) và AABW (Asia AWS Builders Workshop) để có một tuần build nhanh các sản phẩm **agentic AI** chạy trên AWS.
- Đẩy nhanh vòng lặp **build → demo → feedback**: teams pitch trước panel và cộng đồng.
- Khuyến khích tích hợp nhiều AWS service trong cùng một sản phẩm (không chỉ một model isolated).

### Các nhóm & sản phẩm

#### 1. Team 3KA — *Hackathon Journey* (Huỳnh An Khương, Nguyễn Quốc Huy, Ngô Quang Khôi, Hoàng Lê Thành Đức, Đặng Nguyễn Phước Lộc, Đặng Trường Hưng)

Một câu chuyện hành trình tham gia hackathon của chính các thành viên nhóm.

**Key takeaways:**
- Slide deck nhóm không có text chi tiết, chủ yếu là narrative cá nhân.
- Giá trị mang lại: nhắc nhở rằng **storytelling** quan trọng ngang technical depth — kể cả khi slide "nhẹ" về mặt kỹ thuật.

**Tài liệu:**
- Slide: `AWS/FCAJ x AABW/Hackathon_Journey_3KA.pptx`


#### 2. One Team — *AI-Powered Conversation Ordering* (Anh Duy, Tran Dong, Doan Trung, Minh Viet, Anshul Roy)

Sản phẩm AI giúp **sắp xếp cuộc hội thoại** theo mức độ ưu tiên / chủ đề.

**Key takeaways:**
- **Pitch flow gọn**: Trigger → Problem → Product. Dễ nhớ, dễ áp dụng cho demo cuối kỳ.
- Bài toán thực tế: hội thoại dài, nhiều thread, người dùng không muốn đọc lại từ đầu.
- AI dùng làm **filter + reorder**, không thay thế toàn bộ cuộc hội thoại.

**Tài liệu:**
- Slide: `AWS/FCAJ x AABW/OneTeam_CommunityDay.pptx`


#### 3. Plan V — *Solution Architect Professional Native App* (Phạm Tiến Thuận Phát, Huỳnh Hoàng Long, Lê Minh Nghĩa, Trần Đại Vi, Nguyễn An)

Một **AI assistant** cho Solution Architect: từ requirement ngôn ngữ tự nhiên → architecture options + diagram + cost estimate.

**Key takeaways:**
- **Vấn đề rất thực**: SA nhận yêu cầu "làm hệ thống AI cho SOP, có Thursday" — phải tự đọc BRD, vẽ architecture từ đầu trong vài giờ.
- **Giải pháp**: AI assistant giúp:
  - Phân tích requirement ngôn ngữ tự nhiên.
  - Draft architecture options (hybrid-cloud, chuẩn công ty).
  - Sinh **Drawio diagram** + **AWS Architecture Icons**.
  - Ước lượng **cost cho `ap-southeast-1`**.
  - **Refine** qua chat sidebar với custom instructions per project.
- Workflow & architecture demo trực tiếp.

**Tài liệu:**
- Slide: `AWS/FCAJ x AABW/SA_Professional_Native_App.pptx`


#### 4. Signal Scout — *Corporate strategy early detection* (Lê Tấn Lực, Đỗ Hoàng Hiếu, Triệu Quốc Hào, Nguyễn Văn Duy Khiêm, Nguyễn Công Minh, Nguyễn Trần Minh Quân)

Sản phẩm phát hiện **sớm các thay đổi chiến lược doanh nghiệp** (restructuring signals) cho nhóm Enterprise Risk Management, Competitive Intelligence, B2B account management.

**Key takeaways:**
- **Key partners**: AWS, LangFuse, TinyFish, Apify.
- **Value propositions**:
  - Detect restructuring signals.
  - Analyze metrics & build scenarios.
  - Dashboard cho quyết định.
  - **Evidence-backed** — mọi suy luận đều có nguồn trích dẫn.
- **Customer segments**: Enterprise risk management, Competitive intelligence, B2B account management.
- Đề xuất **2 architecture variants**: tiêu chuẩn và cost-efficient — đúng pattern "design for cost first, scale up as needed".

**Tài liệu:**
- Slide: `AWS/FCAJ x AABW/SignalScout.pptx`

### Những gì mình học được từ event này

- **Plan V** giải quyết pain point rất thực tế của SA — đáng học hỏi cách họ kết hợp LLMs với Drawio + AWS Architecture Icons + cost estimation trong cùng một workflow. Liên quan trực tiếp tới đồ án vì **architecture diagram** và **cost guardrail** là 2 deliverable cốt lõi.
- **Signal Scout** minh họa sức mạnh của **evidence-backed decisions** và **partner-aware architecture** (LangFuse + TinyFish + Apify). Hai variant (tiêu chuẩn vs cost-efficient) của họ vang vọng constraint 200 USD của mình — pattern hữu ích: **thiết kế cost-efficient path ngay từ đầu**, sau đó đề xuất standard path như tuỳ chọn nâng cấp.
- **One Team**'s pitch flow (Trigger → Problem → Product) ngắn gọn và dễ nhớ. Mình sẽ **tái sử dụng cấu trúc này** khi demo API heart-attack-risk ở presentation cuối kỳ.
- **Team 3KA** nhắc mình rằng **storytelling** quan trọng ngang technical depth — kể cả khi slide deck "nhẹ" về mặt kỹ thuật.
- **Cross-cutting takeaway**: cả 4 nhóm đều dựa nhiều vào generative AI, nhưng **differentiator là workflow integration** (parse → draft → diagram → estimate → refine). Generic chatbots là baseline; **verticalized assistants** thắng.

### Áp dụng vào đồ án

- Khi demo capstone, mình sẽ dùng pitch flow của One Team: **Trigger (vấn đề latency real-time endpoint vượt budget) → Problem (200 USD cap) → Product (Serverless Inference + Drift Detection + Cost guardrail)**.
- Lấy cảm hứng từ Signal Scout: thiết kế **cost-efficient path** trước (single region, no GPU, serverless) → rồi mới cân nhắc variant tiêu chuẩn (multi-AZ, GPU spot) nếu có budget upgrade. Đây là cách trình bày trade-off rõ ràng cho mentor khi review.
- Plan V's pattern (LLM parses requirement → sinh artifact) gợi ý mình **tách nhỏ inference script** thành các bước có thể tái sử dụng: parse request → preprocess → predict → assemble response — dễ test độc lập, dễ swap từng bước.

### Một số hình ảnh khi tham gia sự kiện

*Thêm ảnh vào thư mục `images/` rồi chèn tại đây. Gợi ý:*
*Drop photos into the `images/` folder and embed them here. Suggested names:*
- `/fcaj-hcmut-template/images/blog/meetup-3-1.jpg` 
- `/fcaj-hcmut-template/images/blog/meetup-3-2jpg.jpg` 
- `/fcaj-hcmut-template/images/blog/meetup-3-3jpg.jpg` 
- `/fcaj-hcmut-template/images/blog/meetup-3-4.jpg` 

> Tổng thể, hackathon cho thấy **một tuần build có thể ra sản phẩm thật** — và đó cũng chính là cách mình muốn vận hành giai đoạn cuối của đồ án capstone.

