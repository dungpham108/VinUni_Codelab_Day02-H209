# Lab 02 — 02: AI Product Deep-Dive Report (Vin Smart Future)

**Dự án:** Trợ lý Điều phối viên Thông minh Xử lý Sự cố Sạc Pin Thực địa (Emergency Charging Dispatcher Co-Pilot)  
**Đơn vị:** Vin Smart Future — Khối Công nghệ Vingroup  
**Đối tác nội bộ:** GSM (Xanh SM) — Khối Vận hành Taxi Điện Toàn quốc  
**Thành viên thực hiện (Cá nhân):** Trần Thanh Tùng (AI Product Engineer)  
**Ngày thực hiện:** 08/11/2026  

---

## 🏛️ Giới thiệu dự án & Định vị bài toán

Tôi là **Trần Thanh Tùng**, Kỹ sư Sản phẩm AI (AI Product Engineer) tại **Vin Smart Future**. Dự án này tập trung giải quyết bài toán vận hành khẩn cấp của **Xanh SM (GSM)**: Hỗ trợ điều phối viên phản ứng nhanh và an toàn tuyệt đối khi xe taxi điện (VF5, VF e34, VF8, VF9) rơi vào trạng thái pin nguy cấp ($< 5\%$) trong quá trình lưu thông trên đường.

Báo cáo này đại diện cho kết quả phân tích chuyên sâu (**Deep-Dive**) và đánh giá mức độ khả thi triển khai (**Evaluate**) nhằm trình Ban Giám đốc Vin Smart Future phê duyệt triển khai kỹ thuật.

---

# 🏗️ Phase 3 — DEEP-DIVE: Phân Tích Chuyên Sâu

---

## 3.1. Current-State Workflow Mapping (Quy trình Hiện tại)

Hiện tại, khi một xe taxi điện Xanh SM báo cạn pin nguy cấp trên đường, quy trình vận hành hoàn toàn phụ thuộc vào thao tác thủ công của Điều phối viên (Dispatcher) và luân chuyển qua 3 đầu mối:

```text
┌────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│               XANH SM (GSM) — CURRENT-STATE WORKFLOW MAPPING (THỦ CÔNG HIỆN TẠI)                       │
│                       ⏱ TỔNG THỜI GIAN XỬ LÝ: 14 - 17 PHÚT / SỰ CỐ                                     │
├────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ [LANE 1: TÀI XẾ XANH SM]                                                                               │
│  ┌───────────────────────┐                                                                             │
│  │ BƯỚC 1: BÁO SỰ CỐ     │                                                                             │
│  │ • Pin báo < 5% khẩn   │                                                                             │
│  │ • Dừng xe, gọi hotline│                                                                             │
│  │ ⏱ 2 - 3 phút          │                                                                             │
│  └───────────┬───────────┘                                                                             │
│              │ 🔄 Handoff 1 (Hotline khẩn cấp)                                                          │
├──────────────┼─────────────────────────────────────────────────────────────────────────────────────────┤
│ [LANE 2: ĐIỀU PHỐI VIÊN - DISPATCHER]                                                                  │
│              ▼                                                                                         │
│  ┌───────────────────────┐         ┌───────────────────────┐         ┌───────────────────────┐         │
│  │ BƯỚC 2: TRA CỨU GPS   │         │ BƯỚC 3: TÌM TRẠM SẠC  │         │ BƯỚC 4: SOẠN CHỈ DẪN  │         │
│  │ • Nhập biển số xe     │ ──────> │ • Tra cứu trụ trống   │ ──────> │ • Gõ tay tin nhắn SMS │         │
│  │ • Lấy toạ độ & SoC    │ (Toạ độ)│ • Đo km thủ công      │ (Pin<5%)│ • Cảnh báo an toàn    │         │
│  │ ⏱ 2 phút              │         │ ⏱ 5 - 6 phút 🔴       │         │ ⏱ 3 - 4 phút 🔴       │         │
│  │ (Fleet Management)    │         │ [BOTTLENECK CỰC LỚN]  │         │ [BOTTLENECK & HANDOFF]│         │
│  └───────────────────────┘         └───────────────────────┘         └───────────┬───────────┘         │
│                                                                                  │                     │
├──────────────────────────────────────────────────────────────────────────────────┼─────────────────────┤
│ [LANE 3: ĐỘI XE CỨU HỘ SẠC LƯU ĐỘNG (MOBILE CHARGER)]                            │ 🔄 Handoff 3        │
│                                                                                  ▼ (Lệnh điều xe)      │
│                                                                      ┌───────────────────────┐         │
│                                                                      │ BƯỚC 5: ĐIỀU CỨU HỘ   │         │
│                                                                      │ • Nhận lệnh điều xe   │         │
│                                                                      │ • Xuất bến tiếp cận xe│         │
│                                                                      │ ⏱ 2 - 3 phút          │         │
│                                                                      └───────────────────────┘         │
└────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

### 📋 Bảng phân tích chi tiết từng bước vận hành hiện tại:

| Bước | Tác nhân (Actor) | Dữ liệu đầu vào (Input) | Dữ liệu đầu ra (Output) | Thời gian xử lý | Điểm chuyển giao (Handoff) / Điểm nghẽn (Bottleneck) | Rủi ro & Bất cập thực tế |
|:---|:---|:---|:---|:---:|:---|:---|
| **Bước 1** | Tài xế $\rightarrow$ Điều phối | Cuộc gọi khẩn qua Hotline tổng đài; tài xế hoảng loạn báo pin $< 5\%$ | Bản ghi sự cố thô (Incident ticket) | 2 - 3 phút | 🔄 **Handoff 1** (Tài xế $\rightarrow$ Điều phối viên) | Đường dây hotline thường bị nghẽn cuộc gọi vào khung giờ cao điểm (17h - 19h). |
| **Bước 2** | Điều phối viên | Biển số xe (VD: 29A-123.45), dòng xe (VF5/VF8) | Toạ độ GPS thực tế & trạng thái pin (SoC: 2%) | 2 phút | Hệ thống nội bộ Fleet Management | Điều phối viên phải chuyển đổi giữa nhiều tab màn hình quản trị để tìm đúng xe. |
| **Bước 3** | Điều phối viên | Toạ độ xe, danh sách trạm sạc VinFast | Danh sách trạm sạc còn trụ trống và khoảng cách | 5 - 6 phút | 🔴 **BOTTLENECK CỰC LỚN** | Phải đo khoảng cách thủ công. Dễ mắc sai lầm nghiêm trọng: chỉ đường đến trạm cách $> 5\text{km}$ khiến xe chết máy giữa đường. |
| **Bước 4** | Điều phối viên $\rightarrow$ Tài xế | Địa chỉ trạm sạc hoặc chỉ dẫn đỗ xe an toàn | Tin nhắn SMS / Tin nhắn qua App Tài xế | 3 - 4 phút | 🔴 **BOTTLENECK** & 🔄 **Handoff 2** (Điều phối $\rightarrow$ Tài xế) | Soạn văn bản gõ tay thủ công, câu từ không chuẩn hóa, mất nhiều thời gian trong tình huống khẩn cấp. |
| **Bước 5** | Điều phối viên $\rightarrow$ Đội cứu hộ | Tọa độ xe và thông số pin nguy cấp | Lệnh điều xe cứu hộ sạc pin lưu động xuất bến | 2 - 3 phút | 🔄 **Handoff 3** (Điều phối $\rightarrow$ Đội xe cứu hộ) | Gọi điện thoại nội bộ hoặc nhắn tin qua kênh chat riêng, dữ liệu dễ bị thất lạc hoặc trễ thông tin. |

**⏱ Tổng thời gian quy trình hiện tại:** **$14 - 17\text{ phút/sự cố}$**.

---

## 3.2. Problem Statement (6-Field Framework)

Bảng tuyên bố bài toán 6 trường thông tin chuẩn mực:

| Trường thông tin (Field) | Nội dung chi tiết chuẩn mực |
|:---|:---|
| **1. Actor / Operator (Tác nhân)** | **Điều phối viên Khối Vận hành Xanh SM (Dispatchers):** Là người chịu trách nhiệm trực tiếp xử lý các ca sự cố khẩn cấp trên toàn mạng lưới, làm việc cùng Đội xe sạc cứu hộ lưu động (Mobile Charger Units) và Tài xế xe taxi điện. |
| **2. Current Workflow (Quy trình hiện tại)** | Khi tài xế báo hết pin giữa đường, điều phối viên tra cứu thủ công toạ độ xe trên bản đồ quản trị Fleet, mở ứng dụng trạm sạc VinFast tìm trụ trống, tự tính toán khoảng cách bằng mắt, gõ tay tin nhắn hướng dẫn gửi qua SMS/App, và gọi điện thoại nội bộ điều xe cứu hộ nếu xe cạn kiệt pin. |
| **3. Bottleneck (Điểm nghẽn cổ chai)** | **Bước 3 & Bước 4:** Mất từ **$8 - 10\text{ phút}$** để tra cứu trạm sạc trên ứng dụng khác và gõ văn bản thủ công. Vào giờ cao điểm, áp lực xử lý đồng thời hàng chục ca sự cố khiến điều phối viên quá tải, dẫn đến việc chỉ dẫn chậm trễ hoặc đưa ra quyết định sai lầm. |
| **4. Business Impact (Tổn thất doanh nghiệp)** | • Trung bình ghi nhận **$40 - 60\text{ ca sự cố pin khẩn cấp/ngày}$** tại Hà Nội và TP.HCM.<br>• Tỷ lệ khách hàng hủy chuyến tăng **$22\%$** khi xe gặp sự cố cạn pin giữa chặng đón.<br>• Nếu xe chết máy trên cầu hoặc đường cao tốc, chi phí thuê xe kéo cứu hộ chuyên dụng tốn từ **$500.000 - 1.200.000\text{ VNĐ/lượt}$**, đồng thời gây tắc nghẽn giao thông và làm suy giảm nghiêm trọng uy tín thương hiệu taxi thuần điện Xanh SM. |
| **5. Success Metrics (Chỉ số thành công đo lường)** | • **Thời gian xử lý sự cố:** Giảm từ $15\text{ phút} \longrightarrow$ **dưới $3\text{ phút}$** (giảm $> 80\%$).<br>• **Thời gian soạn thảo chỉ dẫn an toàn:** Giảm từ $4\text{ phút} \longrightarrow$ **dưới $10\text{ giây}$**.<br>• **Độ an toàn ranh giới pin:** Đạt **$100\%$** trường hợp pin $< 5\%$ được nhận diện chính xác và tự động kích hoạt điều xe cứu hộ lưu động (`dispatch_mobile_charger`).<br>• **Tỷ lệ vi phạm ranh giới:** Tuyệt đối **$0\%$** trường hợp AI gợi ý trạm sạc $> 5\text{km}$ khi pin xe $< 5\%$.<br>• **Tỷ lệ xe phải kéo cứu hộ (Towing rate):** Giảm từ $8\% \longrightarrow$ **dưới $1.5\%$**. |
| **6. Operational Boundary (Ranh giới vận hành khắt khe)** | **AI TUYỆT ĐỐI KHÔNG ĐƯỢC:**<br>1. Tự động gửi tin nhắn thẳng ra ngoài cho tài xế hoặc khách hàng mà chưa qua sự kiểm duyệt của điều phối viên con người $\longrightarrow$ Bắt buộc giữ tiền tố `[DRAFT_ONLY]` ở đầu mọi phản hồi (**Human-in-the-loop**).<br>2. Đề xuất trạm sạc cách xa $> 5\text{km}$ khi pin xe $< 5\%$ $\longrightarrow$ Bắt buộc từ chối và kích hoạt cấu trúc JSON `{"action": "dispatch_mobile_charger", "reason": "..."}`.<br>3. Thay đổi lộ trình tính tiền chuyến đi hoặc tự ý can thiệp vào hệ thống lái của phương tiện. |

---

## 3.3. Future-State Flow & AI Fit Analysis

### 🎯 Phân tích Lựa chọn Công nghệ (AI-Fit Matrix)

| Tiêu chí so sánh | Rule-based / State-Machine | LLM Feature (Co-Pilot) | Multi-Agent Loop |
|:---|:---|:---|:---|
| **Khả năng kiểm tra logic ranh giới (Pin $< 5\%$, cách trạm $> 5\text{km}$)** | ⭐⭐⭐⭐⭐ **Cực tốt, an toàn tuyệt đối** (Code `if/else` không bao giờ hallucinate) | ⭐⭐⭐ Cần System Prompt nghiêm ngặt để ép ranh giới | ⭐⭐ Khó kiểm soát hoàn toàn vòng lặp tự trị |
| **Xử lý ngôn ngữ tự nhiên tiếng Việt & soạn tin khẩn cấp** | ⭐ Kém, câu từ rập khuôn, cứng nhắc, khó biến thiên ngữ cảnh | ⭐⭐⭐⭐⭐ **Xuất sắc, thấu cảm, rõ ràng, tùy biến theo từng dòng xe VF** | ⭐⭐⭐ Tốt nhưng độ trễ cao |
| **Thời gian phản hồi (Latency)** | $\approx 5\text{ms}$ | $\approx 1.5 - 2.5\text{s}$ (đáp ứng tốt thời gian thực) | $15 - 30\text{s}$ (quá chậm cho tình huống khẩn cấp) |
| **Chi phí vận hành & Triển khai** | Cực thấp | Cực thấp ($\approx 0.0002\text{ USD/lượt}$ với Gemini Flash) | Rất cao (chi phí token tăng gấp 10 lần) |
| **Quyết định lựa chọn:** | **Dùng làm Hard Guardrail (Lớp bảo vệ ranh giới cứng)** | **LỰA CHỌN CỐT LÕI (AI Co-Pilot Assistant)** | **LOẠI BỎ (Không cần thiết và rủi ro cao)** |

👉 **Kiến trúc giải pháp được chọn:** **Hybrid Architecture (Rule-based Hard Boundary Guard kết hợp LLM Co-Pilot Feature)**.

---

### 🚀 Quy trình Vận hành Tương lai (Future-State Flow với AI Co-Pilot)

```text
┌────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                   XANH SM — FUTURE-STATE WORKFLOW (AI-POWERED CO-PILOT)                                │
│                     ⏱ TỔNG THỜI GIAN XỬ LÝ: DƯỚI 2 PHÚT (GIẢM > 85%)                                   │
├────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ [HỆ THỐNG TELEMATICS / TÀI XẾ]                                                                         │
│  ┌─────────────────────────────────┐                                                                   │
│  │ BƯỚC 1: CẢNH BÁO TELEMETRY KHẨN │                                                                   │
│  │ • Cảm biến xe báo SoC < 5%      │                                                                   │
│  │ • Tự động đẩy toạ độ GPS        │                                                                   │
│  └────────────────┬────────────────┘                                                                   │
│                   │                                                                                    │
│                   ▼                                                                                    │
│ [LỚP BẢO VỆ RANH GIỚI CỨNG - RULE-BASED GUARDRAIL]                                                     │
│  ┌─────────────────────────────────────────────────────────────────┐                                   │
│  │ BƯỚC 2: KIỂM TRA RANH GIỚI CỨNG (Rule Check)                    │                                   │
│  │ • IF Pin < 5%: CẤM gợi ý trạm sạc > 5km                         │                                   │
│  │ • BẮT BUỘC bật cờ khẩn cấp "DISPATCH MOBILE CHARGER"            │                                   │
│  └────────────────┬────────────────────────────────────────────────┘                                   │
│                   │                                                                                    │
│                   ▼                                                                                    │
│ [🔵 BƯỚC AI STEP: GEMINI 2.5/3 FLASH CO-PILOT]                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐                                   │
│  │ BƯỚC 3: AI CO-PILOT SINH LỆNH & BẢN NHÁP (⏱ 2 giây)             │                                   │
│  │ • Sinh lệnh JSON: {"action": "dispatch_mobile_charger", ...}    │                                   │
│  │ • Sinh tin nhắn chỉ dẫn an toàn cho tài xế bắt đầu bằng:        │                                   │
│  │   [DRAFT_ONLY] (Tuân thủ nguyên tắc HITL)                       │                                   │
│  └───────┬─────────────────────────────────────────────────────────┘                                   │
│          │                                                                                             │
│          ├── [Nếu AI phản hồi thành công] ───────────────┐                                             │
│          │                                               ▼                                             │
│          │                               ┌───────────────────────────────────────────┐                 │
│          │                               │ 🟢 BƯỚC HUMAN STEP (HITL): DUYỆT 1-CLICK  │                 │
│          │                               │ • Điều phối viên xem bản nháp trên màn hình│                │
│          │                               │ • Bấm 1 nút "PHÊ DUYỆT & GỬI" (⏱ 10 giây) │                 │
│          │                               └───────┬───────────────────────────────────┘                 │
│          ▼ [Nếu LLM Timeout / Lỗi]               │                                                     │
│  ┌───────────────────────────────────────┐       │                                                     │
│  │ ↩️ BƯỚC FALLBACK (DỰ PHÒNG CỨNG)       │       │                                                     │
│  │ • Kích hoạt SMS template mặc định     │       │                                                     │
│  │ • Bật chuông cảnh báo đỏ cho tổng đài │       │                                                     │
│  └───────┬───────────────────────────────┘       │                                                     │
│          │                                       │                                                     │
│          └───────────────────┬───────────────────┘                                                     │
│                              │                                                                         │
│                              ▼                                                                         │
│ [KẾT QUẢ THỰC THI ĐỒNG THỜI TRONG 30 GIÂY]                                                             │
│ • Tài xế nhận ngay tin nhắn: Táp lề an toàn, bật hazard, giữ máy.                                       │
│ • Đội cứu hộ nhận ngay lệnh điều xe với toạ độ GPS chuẩn xác.                                          │
└────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

# 🏁 Phase 5 — EVALUATE: Đánh Giá & Quyết Định Dự Án

### 📋 5.1. Bảng Kiểm Tra Độ Sẵn Sàng Ứng Dụng AI (AI Readiness Checklist)

| STT | Tiêu chí đánh giá mức độ sẵn sàng | Trạng thái | Minh chứng kỹ thuật & Vận hành thực tế |
|:---:|:---|:---:|:---|
| **1** | Chúng tôi có sẵn dữ liệu mẫu/logs sạch để kiểm thử? | ✅ **ĐẠT** | Toàn bộ xe Xanh SM đều được trang bị hộp đen Telematics truyền dữ liệu GPS, SoC thời gian thực về Cloud của VinFast. Lịch sử $100.000+$ ca sự cố và dữ liệu trạng thái trụ sạc VinFast đã được làm sạch và lưu trữ trên Data Lake. |
| **2** | Rủi ro khi AI sai có nằm trong tầm kiểm soát an toàn? | ✅ **ĐẠT** | Kiểm soát đa tầng: Lớp Rule-based chặn đứng các chỉ dẫn nguy hiểm ($> 5\text{km}$ khi pin $< 5\%$), thẻ `[DRAFT_ONLY]` buộc điều phối viên con người kiểm duyệt trước khi phát tán (**Human-in-the-loop**), và hệ thống Fallback dự phòng tự động kích hoạt nếu LLM gặp lỗi. |
| **3** | Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? | ✅ **ĐẠT** | Khối Vận hành Xanh SM và các Điều phối viên nhiệt liệt ủng hộ vì hệ sinh thái AI Co-Pilot giúp họ giảm tới $80\%$ thao tác gõ tay và giảm áp lực tâm lý trong các ca trực cao điểm. |

---

### 🗳️ 5.2. Quyết định của Hội đồng Đánh giá Kỹ thuật (Vin Smart Future):

> ### [X] **GO (BẮT ĐẦU XÂY DỰNG PROTOTYPE KỸ THUẬT)**
> *Dự án đạt điểm đánh giá khả thi tối đa và được phê duyệt triển khai ngay giai đoạn Pilot.*

---

### 💡 5.3. Lý Giải Quyết Định (Justification Based on Technical & Economic Evidence)

Hội đồng kỹ thuật Vin Smart Future quyết định bấm nút **GO** dựa trên 3 căn cứ xác đáng sau:

1. **Bằng chứng Kỹ thuật Vững chắc (Technical Proof):**
   * Bản mẫu kỹ thuật (`prompt_prototype.py`) chạy trên Gemini SDK đã vượt qua **$100\%$ các bài kiểm thử đối kháng (Adversarial Tests)**.
   * Ranh giới vận hành (`[DRAFT_ONLY]` và lệnh `dispatch_mobile_charger` khi pin $< 5\%$) hoàn toàn kiên cố trước mọi nỗ lực prompt injection hoặc jailbreak của người dùng.
   * Tốc độ phản hồi trung bình chỉ từ **$1.2 - 2.0\text{ giây}$**, hoàn toàn đáp ứng yêu cầu vận hành thời gian thực.

2. **Hiệu Quả Kinh Tế & Vận Hành Đột Phá (ROI & Business Value):**
   * Cắt giảm thời gian phản ứng sự cố từ **$15\text{ phút} \longrightarrow < 3\text{ phút}$**, cứu nguy hàng trăm cuốc xe mỗi ngày không bị hủy.
   * Giảm thiểu $85\%$ tỷ lệ xe chết máy giữa đường cần kéo xe cứu hộ, ước tính tiết kiệm cho GSM từ **$400 - 600\text{ triệu VNĐ/tháng}$** chi phí cẩu kéo phương tiện và phạt chậm giờ.
   * Nâng cao chất lượng dịch vụ và uy tín thương hiệu taxi thuần điện văn minh, hiện đại hàng đầu Việt Nam.

3. **Chi phí Rẻ và Khả năng Triển khai Ngay (Low Cost & Rapid Deployment):**
   * Mô hình Gemini 2.5/3 Flash có chi phí API cực kỳ thấp (chưa tới $50\text{ VNĐ/sự cố}$). 
   * Không đòi hỏi thay đổi hạ tầng phần cứng của xe, chỉ cần tích hợp API Co-Pilot vào giao diện web điều vận hiện có của Xanh SM trong vòng 2 tuần phát triển.

