# Lab 02 — 01: Problem Scan & Quick Assessment (Vin Smart Future)

**Đơn vị:** Vin Smart Future — Khối Công nghệ Vingroup  
**Thành viên thực hiện (Cá nhân):** Trần Thanh Tùng (AI Product Engineer)  
**Phân nhánh dự án:** Xanh SM (GSM - Green & Smart Mobility)  
**Ngày thực hiện:** 08/11/2026  

---

## 🏛️ Bối cảnh & Vai trò cá nhân

Tôi là **Trần Thanh Tùng**, Kỹ sư Sản phẩm AI (AI Product Engineer) tại **Vin Smart Future**. Trong dự án này, tôi đóng vai trò là thành viên trong nhóm nghiên cứu, phối hợp trực tiếp với Khối Vận hành của **GSM (Xanh SM)** để phát hiện các điểm nghẽn (bottlenecks) trong quy trình điều vận đội xe taxi điện tại các thành phố lớn (Hà Nội, TP.HCM). 

Mục tiêu của tôi là ứng dụng **4 Lenses** để quét toàn diện hệ sinh thái Vingroup, đánh giá nhanh độ khả thi của các cơ hội ứng dụng AI, và chọn ra bài toán có giá trị thực tế cao nhất để làm việc cùng nhóm.

---

# 🔍 Phase 1 — SCAN: Bảng Quét Cơ Hội (Opportunity Scan Table)

Áp dụng **4 Lenses tìm bài toán AI** (Lặp lại, Tốn thời gian, AI có thể làm tốt hơn, Pain từ người khác) để khảo sát hoạt động vận hành xuyên suốt các công ty thành viên Vingroup:

| # | Đơn vị thành viên (Subsidiary) | Ống kính (Lens) | Mô tả ngắn bài toán & Điểm nghẽn thực tế | Tổn thất ước tính (Pain / Metric) |
|---|--------------------------------|-----------------|------------------------------------------|-----------------------------------|
| **1** | **Xanh SM (GSM)** | **Tốn thời gian (Time-consuming)** | **Xử lý sự cố hết pin nguy cấp (< 5%) trên đường:** Điều phối viên phải kiểm tra thủ công toạ độ xe trên bản đồ, gọi điện hỏi tình trạng pin, tra cứu trạm sạc VinFast còn trụ trống, hoặc gọi điều xe cứu hộ lưu động. | Mất 15 - 20 phút/sự cố; nguy cơ xe chết máy giữa đường gây ùn tắc giao thông và giảm SLA phục vụ khách. |
| **2** | **Xanh SM (GSM)** | **Pain từ người khác (Stakeholder Pain)** | **Tối ưu điểm đón khách dựa trên tin nhắn chỉ đường:** Khách hàng nhắn tin mô tả điểm đón phức tạp (ví dụ: *"sảnh sau tháp S2.05 Vinhomes Ocean Park đối diện quán cafe"*), tài xế vừa lái xe vừa phải đọc hiểu tin nhắn và gọi lại hỏi đường gây phàn nàn và nguy cơ va chạm. | Hơn 18% cuốc xe giờ cao điểm bị trễ đón > 7 phút; tài xế bị áp lực tâm lý và phàn nàn về app. |
| **3** | **VinFast** | **AI có thể tốt hơn (AI-upgrade)** | **Chẩn đoán sơ bộ mã lỗi xe từ mô tả tiếng Việt của chủ xe:** Khách hàng gọi hotline báo hiện tượng lạ (ví dụ: *"vào cua có tiếng lạch cạch ở trục sau"*), nhân viên CSKH không có chuyên môn cơ khí khó phân loại chính xác mã lỗi kỹ thuật ban đầu để đặt lịch xưởng. | 35% lượt đặt lịch xưởng dịch vụ bị xếp sai chuyên khoa, khiến xưởng thiếu phụ tùng thay thế kịp thời. |
| **4** | **Vinhomes** | **Lặp lại (Repetitive)** | **Phân loại và định tuyến phản ánh của cư dân:** Nhân viên tiếp nhận hàng trăm ticket phản ánh qua Vinhomes Resident App mỗi ngày (tiếng ồn, đèn hành lang hỏng, rò rỉ nước, đăng ký thi công) và gán tay thủ công cho từng bộ phận ban quản lý. | Mất 2 - 4 tiếng để ticket đến đúng kỹ thuật viên tòa nhà; tỷ lệ phân loại sai bộ phận chiếm 12%. |
| **5** | **Vinmec** | **Tốn thời gian (Time-consuming)** | **Soạn thảo tóm tắt hồ sơ xuất viện (Discharge Summary):** Bác sĩ điều trị mất rất nhiều thời gian tổng hợp kết quả xét nghiệm, lịch sử dùng thuốc và chẩn đoán để viết tóm tắt ra viện bằng ngôn ngữ dễ hiểu cho bệnh nhân và người nhà. | Mất 20 - 30 phút/bệnh nhân xuất viện; bác sĩ quá tải hồ sơ giấy tờ làm giảm thời gian khám bệnh trực tiếp. |
| **6** | **Xanh SM (GSM)** | **Pain từ người khác (Stakeholder Pain)** | **Phân tích lý do khách hủy cuốc xe (Trip Cancellation Pattern):** Bộ phận vận hành phải nghe lại hàng nghìn đoạn ghi âm cuộc gọi và ghi chú huỷ chuyến của tài xế để tìm nguyên nhân gốc rễ (do app định vị sai, do trạm sạc, hay do thời tiết). | Phân tích chậm trễ hàng tuần; tổn thất doanh thu do không kịp thời phát hiện các điểm đen điều vận. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Thẻ Bài Toán Tiềm Năng (Quick Problem Cards)

Chọn ra **Top 3 bài toán** tiềm năng nhất từ danh sách trên để đánh giá chi tiết:
1. **Card #1:** Xanh SM — Trợ lý ảo điều phối cứu hộ & xử lý xe điện cạn pin khẩn cấp
2. **Card #2:** Xanh SM — Trích xuất toạ độ và tối ưu điểm đón khách từ tin nhắn mô tả
3. **Card #3:** Vinhomes — Hệ thống phân loại & định tuyến ticket phản ánh cư dân tự động

---

### 🎴 QUICK PROBLEM CARD #1: Xanh SM — Trợ Lý Điều Phối Cứu Hộ & Xử Lý Sự Cố Sạc Pin Thực Địa

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1 (LỰA CHỌN ĐỂ DEEP-DIVE)                                          │
│                                                                                        │
│ Bài toán (1 câu): Hỗ trợ điều phối viên Xanh SM ra quyết định khẩn cấp và soạn chỉ     │
│ dẫn an toàn khi xe taxi điện rơi vào mức pin nguy cấp (< 5%) trên đường đón khách.      │
│                                                                                        │
│ Công ty thành viên: [x] Xanh SM (GSM)     [ ] VinFast     [ ] Vinhomes                 │
│                     [ ] Vinmec            [ ] Khác: ___________________                │
│                                                                                        │
│ Ai đang đau (Actor)?                                                                   │
│ • Điều phối viên Xanh SM (quá tải vì phải tra cứu thủ công toạ độ, trụ sạc, gọi cứu hộ).│
│ • Tài xế Xanh SM (lo lắng xe chết máy giữa đường, bị trừ điểm KPI và phạt vì đón trễ). │
│ • Khách hàng (bị gián đoạn hành trình hoặc bị hủy cuốc xe đột ngột).                   │
│                                                                                        │
│ Workflow thủ công hiện tại (5 bước):                                                   │
│   1. Tài xế gọi hotline khẩn cấp báo xe còn < 5% pin và không thể tiếp tục di chuyển.  │
│   ──> 2. Điều phối viên tra cứu tọa độ xe trên bản đồ quản trị đội xe.                 │
│   ──> 3. Điều phối viên mở hệ thống trạm sạc VinFast tìm trạm gần nhất còn trụ trống. │
│   ──> 4. Nhập tay địa chỉ trạm sạc hoặc gọi cho đội xe cứu hộ sạc lưu động.            │
│   ──> 5. Soạn tin nhắn hướng dẫn/cảnh báo an toàn gửi cho tài xế qua ứng dụng.         │
│                                                                                        │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 10 - 15 phút/sự cố).                   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3, 4 & 5:                                   │
│ • Tự động nhận diện toạ độ & mức pin nguy cấp.                                         │
│ • Kích hoạt ngay lệnh điều xe sạc di động (dispatch_mobile_charger) nếu pin < 5%.      │
│ • Tự động soạn thảo bản nháp hướng dẫn an toàn [DRAFT_ONLY] cho điều phối viên duyệt.  │
│                                                                                        │
│ Đo thành công bằng gì (Metric có số)?                                                  │
│ • Giảm thời gian xử lý toàn bộ sự cố từ 15 phút ──> dưới 3 phút.                       │
│ • 100% trường hợp pin < 5% được chặn chỉ đường trạm xa (> 5km) và gọi xe cứu hộ.     │
│ • Giảm 80% thời gian soạn thảo chỉ dẫn an toàn của điều phối viên (dưới 30 giây).      │
│                                                                                        │
│ Quick Architecture: [x] LLM Feature (kết hợp Rule-based boundary & Human-in-the-loop) │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

### 🎴 QUICK PROBLEM CARD #2: Xanh SM — Trích Xuất Điểm Đón Thông Minh Từ Tin Nhắn

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                                  │
│                                                                                        │
│ Bài toán (1 câu): Tự động trích xuất thực thể vị trí chính xác từ tin nhắn mô tả      │
│ của khách hàng tại các khu đô thị phức tạp để gợi ý điểm đón thuận tiện nhất cho xe.   │
│                                                                                        │
│ Công ty thành viên: [x] Xanh SM (GSM)     [ ] VinFast     [ ] Vinhomes                 │
│                     [ ] Vinmec            [ ] Khác: ___________________                │
│                                                                                        │
│ Ai đang đau (Actor)?                                                                   │
│ • Tài xế Xanh SM (vừa lái xe vừa căng mắt đọc tin nhắn chỉ đường rối rắm của khách).   │
│ • Khách hàng (khó chịu vì tài xế đón nhầm sảnh, nhầm cổng tòa nhà).                    │
│                                                                                        │
│ Workflow thủ công hiện tại (4 bước):                                                   │
│   1. Khách hàng gõ tin nhắn mô tả vị trí ("đứng ở quán Highlands đối diện sảnh S2.01").│
│   ──> 2. Tài xế nhìn thông báo trên điện thoại khi đang di chuyển.                     │
│   ──> 3. Tài xế dừng xe hoặc gọi lại để xác nhận lại vị trí cụ thể.                    │
│   ──> 4. Tài xế tự định vị lại lộ trình trên bản đồ.                                   │
│                                                                                        │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 3 - 5 phút/cuốc đón).                   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3:                                      │
│ • LLM phân tích văn bản tự nhiên, trích xuất POI (Point of Interest) chuẩn.             │
│ • Tự động gắn tag toạ độ GPS chính xác lên bản đồ dẫn đường cho tài xế.                │
│                                                                                        │
│ Đo thành công bằng gì (Metric có số)?                                                  │
│ • Giảm 50% số cuộc gọi tài xế phải gọi lại cho khách để hỏi điểm đón.                  │
│ • Rút ngắn thời gian tiếp cận khách đón từ 6 phút ──> dưới 3 phút.                     │
│                                                                                        │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent                            │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

### 🎴 QUICK PROBLEM CARD #3: Vinhomes — Phân Loại Tự Động & Điều Hướng Phản Ánh Cư Dân

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                                  │
│                                                                                        │
│ Bài toán (1 câu): Phân loại nội dung phản ánh của cư dân qua App Vinhomes Resident    │
│ và tự động điều phối ticket đến đúng tổ kỹ thuật/an ninh của từng phân khu.            │
│                                                                                        │
│ Công ty thành viên: [ ] Xanh SM (GSM)     [ ] VinFast     [x] Vinhomes                 │
│                     [ ] Vinmec            [ ] Khác: ___________________                │
│                                                                                        │
│ Ai đang đau (Actor)?                                                                   │
│ • Nhân viên trực tổng đài Ban Quản lý Vinhomes (phải đọc và gắn tag thủ công hàng trăm │
│   ticket mỗi ngày).                                                                    │
│ • Cư dân Vinhomes (chờ đợi nhiều tiếng đồng hồ mới có phản hồi ban đầu).               │
│                                                                                        │
│ Workflow thủ công hiện tại (4 bước):                                                   │
│   1. Cư dân gửi ticket phản ánh kèm mô tả/ảnh trên app.                                │
│   ──> 2. Nhân viên CSKH đọc nội dung, xác định loại sự cố (kỹ thuật, an ninh, vệ sinh).│
│   ──> 3. CSKH gán nhãn thủ công và chọn người tiếp nhận theo phân khu.                 │
│   ──> 4. Nhân viên kỹ thuật nhận thông báo và đến hiện trường xử lý.                   │
│                                                                                        │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 30 - 60 phút từ lúc gửi đến lúc điều).  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3:                                      │
│ • Phân loại văn bản tự động gán nhãn chuyên môn và độ khẩn cấp (P1, P2, P3).           │
│ • Tự động routing ticket đến kỹ thuật viên đang trực ca gần nhất.                      │
│                                                                                        │
│ Đo thành công bằng gì (Metric có số)?                                                  │
│ • Giảm thời gian định tuyến ticket từ 45 phút ──> dưới 10 giây.                        │
│ • Độ chính xác phân loại sự cố đạt $\ge 93\%$.                                         │
│                                                                                        │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent                            │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🎯 Đánh giá và Quyết định Lựa chọn Bài toán của Nhóm

Sau khi phân tích và đối chiếu 3 thẻ bài toán:
* **Card #2 (Điểm đón Xanh SM):** Giá trị gia tăng tốt nhưng phụ thuộc lớn vào độ chính xác của bản đồ nội khu và dữ liệu POI địa phương (OpenStreetMap/Google Maps API), scope khá rộng cho giai đoạn này.
* **Card #3 (Vinhomes Ticket):** Đây là tác vụ bất đồng bộ (asynchronous / back-office), rủi ro chậm trễ không đe dọa trực tiếp đến tính mạng con người hay an toàn giao thông thời gian thực.
* **Card #1 (Xanh SM Sự cố sạc pin khẩn cấp):** **Được nhóm lựa chọn 100% để thực hiện Deep-Dive**. 
  * **Lý do:** Bài toán ảnh hưởng trực tiếp đến SLA thời gian thực của taxi điện Xanh SM, có rủi ro an toàn phương tiện cao (xe chết máy trên đường có thể gây tai nạn giao thông), có ranh giới vận hành rõ ràng (`[DRAFT_ONLY]` và ngưỡng pin `< 5%`), hoàn toàn khả thi để xây dựng prototype kỹ thuật có Human-in-the-loop.

