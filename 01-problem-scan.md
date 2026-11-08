# 01 — Problem Scan (Phase 1 & 2)

**Tên nhóm:** TODO_TEN_NHOM
**Thành viên (Họ tên — email):**
- Đỗ Minh Hiếu — mhieudo11102@gmail.com
- TODO_HO_TEN — TODO_EMAIL

**Người làm file này:** Đỗ Minh Hiếu — mhieudo11102@gmail.com

> Quy ước: mỗi bài toán đều ghi nguồn. Số liệu có nguồn ghi rõ nguồn. Số liệu do nhóm tự đề xuất ghi là **[Giả định]**. Chỗ chưa có dữ liệu ghi **[Chưa có dữ liệu]**, không bịa số.

---

## Phase 1 — SCAN

**4 Lenses:** Repetitive (lặp lại) · Time-consuming (tốn thời gian) · AI-upgrade (AI làm tốt hơn) · Stakeholder Pain (pain của người khác)

### List bài toán của tôi

| # | Công ty | Lens | Mô tả bài toán (1–2 câu) | Mức bằng chứng |
|---|---------|------|--------------------------|----------------|
| 1 | VinFast | Stakeholder Pain, AI-upgrade | Phát hiện chậm các nhóm lỗi an toàn lặp lại: Customer Care đọc bình luận khách hàng thủ công hằng ngày; có đợt triệu hồi được thúc đẩy bởi cuộc điều tra của cơ quan quản lý (NHTSA). | Có tài liệu gốc [N1, N2, N3] |
| 2 | Vinhomes | Repetitive, Time-consuming | Theo dõi SLA xử lý khiếu nại cư dân: quy định yêu cầu bộ phận tiếp nhận đôn đốc bộ phận xử lý ít nhất 5 ngày/lần, không mô tả công cụ theo dõi. | Có tài liệu gốc [N4]; mức tuân thủ SLA thực tế **chưa có dữ liệu** |
| 3 | VinFast | Time-consuming, Stakeholder Pain | Xe nằm xưởng lâu vì chờ phụ tùng; phụ tùng cho model và thị trường mới thiếu lịch sử hỏng hóc để dự báo. | Có nguồn cho chi phí chờ và cam kết 24h [N5, N6, N7]; nút thắt dự báo là **suy luận của nhóm** |
| 4 | Xanh SM (Green SM) | Time-consuming, Stakeholder Pain | Tài xế mất thời gian xếp hàng sạc ở trạm công cộng giờ cao điểm. | Có tuyên bố của Green SM và khảo sát Hà Nội [N8, N9]; **chưa có số đo độc lập** |
| 5 | V-Green (Khác) | Repetitive | Xử lý sự cố trụ sạc theo cam kết 48 giờ trong mạng lưới hàng chục nghìn trụ, nhiều đối tác vận hành. | Chỉ có bối cảnh [N8, N10]; nút thắt **chưa kiểm chứng** |
| 6 | V-Green (Khác) | AI-upgrade | Chọn vị trí và cân bằng tải trạm sạc: có điểm kín chỗ giờ cao điểm, có trạm lớn không phải chờ. | Chỉ có bối cảnh [N9, N10, N11]; dữ liệu sử dụng **chưa có** |
| 7 | VinFast | Time-consuming | Thẩm định bằng chứng bảo hành pin (ngưỡng thay pin 70% SoH). | Chỉ có điều khoản [N12, N13]; chưa thấy bằng chứng quyết định chậm hoặc thiếu nhất quán, **chưa kiểm chứng** |

---

## Phase 2 — QUICK-ASSESS

Top 3 được chọn: **#1, #2, #3**. Lý do: có tài liệu gốc mô tả quy trình hoặc ngưỡng cam kết (SLA, thời gian giao phụ tùng) để làm mốc đo. Riêng baseline thực tế của #2 và #3 vẫn **[Chưa có dữ liệu]**.

---

### Quick Problem Card 1 — VinFast: cảnh báo sớm từ tín hiệu lỗi thực địa

| Mục | Nội dung |
|-----|----------|
| 1. Bài toán (1 câu) | Nhóm lỗi an toàn lặp lại được phát hiện chậm vì khâu rà soát phản ánh khách hàng là thủ công, và có trường hợp cơ quan quản lý phát hiện trước. |
| 2. Công ty | VinFast |
| 3. Ai đang đau (Actor) | Nhân viên Customer Care và Hội đồng đảm bảo sản phẩm (VPAC) |
| 4. Workflow thủ công hiện tại | (1) Customer Care rà soát bình luận và phản ánh khách hàng hằng ngày. (2) Phát hiện một ca đáng ngờ, chuyển VPAC. (3) VPAC theo dõi hằng ngày các phản ánh tương tự. (4) Đủ số ca, lập nhóm liên chức năng điều tra nguyên nhân. (5) VPAC đề xuất triệu hồi. *(Nguồn: báo cáo triệu hồi 23V-355, N1)* |
| 5. Bước tốn thời gian/lỗi nhất + thời gian mỗi lượt | Tổng **15 ngày** từ tín hiệu đầu tiên (27/4/2023) đến đề xuất triệu hồi (12/5/2023) (N1). Chia theo mốc trong N1: **(a) gom tín hiệu** 27/4 → 2/5 (khoảng 6 ngày, đến khi đủ 5 phản ánh cùng triệu chứng); **(b) khoảng chờ không được mô tả** 3/5 → 7/5 (khoảng 5 ngày, trong đó có lập nhóm liên chức năng); **(c) điều tra kỹ thuật** 8/5 → 11/5 (4 ngày). Bước (a) và (b) chiếm khoảng 11/15 ngày và là phần rà soát, tổng hợp bằng chứng thủ công. Lý do của khoảng chờ (b) **[Chưa có dữ liệu, cần log nội bộ]**. Đơn vị là ngày vì quy trình tính theo ngày. Trường hợp lỗi giữ làn: NHTSA mở điều tra 10/09/2024 từ 14 báo cáo chủ xe (N2), recall 25V-559 được ghi là thúc đẩy bởi một cuộc điều tra của NHTSA, nộp 28/08/2025, khoảng 11,5 tháng sau (N3; trang này không ghi số điều tra, việc nối với PE24025 là suy luận của nhóm dựa trên cùng hệ thống giữ làn). Hai mốc không so sánh trực tiếp được. |
| 6. AI hỗ trợ ở bước nào | Bước 1–3: gom cụm phản ánh theo triệu chứng, trích xuất thông tin, cảnh báo khi một cụm vượt ngưỡng, tóm tắt bằng chứng cho VPAC. |
| 7. Metric thành công (có số) | **Metric chính:** thời gian từ tín hiệu đầu tiên đến khi bắt đầu điều tra kỹ thuật. Baseline 2023: khoảng 11 ngày (27/4 → 8/5), và 15 ngày đến đề xuất triệu hồi (N1). **[Giả định]** mục tiêu thử nghiệm: giảm từ khoảng 11 ngày xuống **≤ 5 ngày**, nhờ cảnh báo cụm lỗi trong **≤ 24 giờ** kể từ khi cụm đạt ngưỡng. **Metric phụ:** back-test trên các vụ recall đã công bố, **[Giả định]** phát hiện được **≥ 80%** cụm lỗi sớm hơn mốc thực tế. Không đặt mục tiêu cho thời gian điều tra kỹ thuật và quyết định triệu hồi, vì phần này vẫn do VPAC làm. |
| 8. Quick Architecture | **LLM feature + Rule** (LLM phân loại và tóm tắt, rule đặt ngưỡng cảnh báo). Không cần Agent. VPAC vẫn là người quyết định triệu hồi (HITL). |

**Giả thuyết cần kiểm chứng:** rà soát thủ công sẽ khó theo kịp khi quy mô tăng. VinFast bàn giao 78.458 ô tô điện trong 4 tháng đầu 2026 (bài báo không ghi rõ thị trường), tăng 75,56% so với cùng kỳ (N14). Đây là suy luận của nhóm, không phải kết luận từ nguồn.

**Phản biện (góc nhìn CFO / Trưởng phòng Vận hành):**
- Năm 2023 đợt recall chỉ liên quan 264 xe đang lưu hành (N1). Với khối lượng nhỏ như vậy, một người đọc thủ công vẫn theo kịp, nên lợi ích của AI chỉ rõ khi quy mô tăng.
- Rule đếm từ khoá (ví dụ "màn hình đen", "blank screen") có thể bắt được triệu chứng **đã biết** với chi phí thấp. LLM chỉ đáng dùng khi phải gom các mô tả tự do, nhiều ngôn ngữ, cho triệu chứng **mới**.
- Cảnh báo sai nhiều sẽ làm VPAC mất thời gian và dần bỏ qua cảnh báo. Cần đo thêm tỷ lệ cảnh báo đúng, không chỉ đo tốc độ.
- Back-test cần dữ liệu phản ánh cũ có gắn nhãn. Hiện **[Chưa có dữ liệu]**.

---

### Quick Problem Card 2 — Vinhomes: theo dõi SLA xử lý khiếu nại cư dân

| Mục | Nội dung |
|-----|----------|
| 1. Bài toán (1 câu) | Việc đôn đốc để khiếu nại cư dân được xử lý đúng hạn dựa vào theo dõi thủ công (ít nhất 5 ngày/lần), nên dễ trễ hạn mà không bị phát hiện. |
| 2. Công ty | Vinhomes |
| 3. Ai đang đau (Actor) | Nhân viên tổng đài và Ban Quản lý (bộ phận tiếp nhận), bộ phận xử lý, CSKH phản hồi; cư dân là người chịu hậu quả |
| 4. Workflow thủ công hiện tại | (1) Tiếp nhận qua hotline, email, thư giấy hoặc trực tiếp. (2) Chuyển bộ phận xử lý: trong 4 giờ làm việc (điện thoại), trong 24 giờ (email, thư giấy). (3) Bộ phận xử lý trình lãnh đạo duyệt phương án rồi giải quyết: yêu cầu đơn giản trong 4 ngày làm việc; yêu cầu phức tạp hẹn phản hồi chính thức trong 6 ngày làm việc; yêu cầu khẩn liên quan an toàn tính mạng xử lý ngay; đến hạn chưa có phương án thì gia hạn với khách. (4) Bộ phận tiếp nhận kiểm tra tiến độ và đôn đốc ít nhất 5 ngày/lần. (5) CSKH phản hồi khách: trong 8 giờ làm việc kể từ khi có kết quả (email/thư), hoặc trong 1 ngày làm việc kèm khảo sát (điện thoại). *(Nguồn: Quy định xử lý khiếu nại của Vinhomes, cập nhật 05/05/2025, N4)* |
| 5. Bước tốn thời gian/lỗi nhất + thời gian mỗi lượt | Bước 3 (4–6 ngày làm việc) và bước 4 (đôn đốc thủ công). Văn bản không mô tả công cụ ghi nhận hồ sơ. **[Chưa có dữ liệu]** về tỷ lệ trễ hạn và thời gian thực tế. **[Giả định]** cần đo thời gian mỗi lượt đôn đốc (ví dụ 10–15 phút/lần là ước tính để nhóm kiểm tra, chưa có nguồn). |
| 6. AI hỗ trợ ở bước nào | Bước 1–2: phân loại và định tuyến. Bước 4: nhắc và đôn đốc tự động theo SLA. Bước 5: soạn nháp phản hồi, tổng hợp báo cáo cho lãnh đạo. |
| 7. Metric thành công (có số) | Ngưỡng SLA lấy từ quy định (N4); baseline hiện tại **[Chưa có dữ liệu]**. **[Giả định]** mục tiêu thử nghiệm: **≥ 95%** hồ sơ điện thoại được chuyển trong ≤ 4 giờ làm việc; **≥ 90%** yêu cầu đơn giản xong trong 4 ngày làm việc và yêu cầu phức tạp được phản hồi chính thức trong 6 ngày làm việc; **100%** hồ sơ sắp quá hạn được cảnh báo trước hạn ít nhất 1 ngày làm việc. |
| 8. Quick Architecture | **Rule / State-machine** (đồng hồ SLA, nhắc việc, leo thang) + **LLM** (phân loại, soạn nháp). Không cần Agent. Lãnh đạo vẫn duyệt phương án; nhóm khách hàng dễ tổn thương được ưu tiên theo quy định. |

**Lưu ý trùng gợi ý:** inspiration kit có gợi ý "định tuyến khiếu nại cư dân", vì vậy bài toán này nhấn mạnh **theo dõi SLA và đôn đốc**, không chỉ phân loại.

**Phản biện (góc nhìn CFO / Trưởng phòng Vận hành):**
- Phần cốt lõi (đồng hồ SLA, nhắc việc, leo thang) là bài toán **rule / state-machine** thuần. Một hệ thống ticket thông thường giải quyết được mà không cần AI. LLM chỉ thêm giá trị ở bước phân loại và soạn nháp.
- Chưa biết Vinhomes đã có hệ thống ticket nội bộ hay chưa **[Chưa có dữ liệu]**. Nếu đã có, phần việc thật sự chỉ là bật cảnh báo SLA.
- Các mục tiêu 95% / 90% là giả định khi chưa có baseline, nên chưa chứng minh được mức cải thiện.

---

### Quick Problem Card 3 — VinFast: phụ tùng và thời gian xe nằm xưởng

| Mục | Nội dung |
|-----|----------|
| 1. Bài toán (1 câu) | Xe nằm xưởng lâu vì chờ phụ tùng, đặc biệt ở model và thị trường mới chưa có lịch sử hỏng hóc để dự báo tồn kho. |
| 2. Công ty | VinFast (hậu mãi) |
| 3. Ai đang đau (Actor) | Planner phụ tùng hậu mãi; cố vấn và kỹ thuật viên xưởng dịch vụ; chủ xe |
| 4. Workflow thủ công hiện tại | **[Suy luận, chưa có nguồn mô tả quy trình nội bộ]** (1) Xe vào xưởng, chẩn đoán. (2) Kiểm kho xưởng. (3) Thiếu thì đặt từ kho trung chuyển. (4) Chờ giao. (5) Sửa và trả xe. |
| 5. Bước tốn thời gian/lỗi nhất + thời gian mỗi lượt | Bước 4 (chờ phụ tùng). Mốc có nguồn: tại Mỹ, theo chính sách công bố 06/2023, VinFast trả 100 USD cho mỗi ngày xe nằm xưởng kể từ ngày thứ 4 khi sửa chữa kéo dài quá 3 ngày, không giới hạn số ngày (N5); cam kết giao phụ tùng trong 24 giờ từ 01/09/2024 (N6). **[Chưa có dữ liệu]** thời gian chờ thực tế theo thị trường. Chính sách chi trả tại Mỹ chưa kiểm chứng còn áp dụng hay không. |
| 6. AI hỗ trợ ở bước nào | Bước 2–3: dự báo nhu cầu phụ tùng cho model và thị trường mới (tận dụng dữ liệu từ model và thị trường tương tự), đề xuất mức tồn kho theo từng xưởng. |
| 7. Metric thành công (có số) | Tỷ lệ lệnh sửa chữa phải chờ phụ tùng quá 3 ngày; tỷ lệ phụ tùng thông dụng giao đúng 24 giờ. Baseline **[Chưa có dữ liệu]**. **[Giả định]** mục tiêu thử nghiệm: **< 10%** lệnh sửa chữa chờ phụ tùng quá 3 ngày; **≥ 95%** phụ tùng thông dụng giao trong 24 giờ (ngưỡng 24 giờ lấy từ cam kết N6). Quy mô thị trường: VinFast bán 21.820 xe ở nước ngoài năm 2025, Indonesia lớn nhất với 10.890 xe (N7). |
| 8. Quick Architecture | **Rule + mô hình thống kê** là baseline (điểm đặt hàng min-max, dự báo theo nhóm tương tự). LLM chỉ có thể dùng để giải thích đề xuất. **Có nguy cơ kết luận NO-GO cho LLM nếu rule-based đủ tốt**, nhóm cần nêu rõ ở Phase 5. |

**Phản biện (góc nhìn CFO / Trưởng phòng Vận hành):**
- Dự báo nhu cầu phụ tùng là bài toán **thống kê / ML dạng bảng**, không phải bài toán ngôn ngữ. Dùng LLM ở đây vừa tốn chi phí vừa khó kiểm chứng.
- Model và thị trường mới thiếu lịch sử hỏng hóc, nên mọi phương pháp dự báo đều yếu, không riêng AI.
- Chưa lượng hoá được đánh đổi giữa chi phí tồn kho và chi phí xe nằm xưởng **[Chưa có dữ liệu]**.

---

### Tóm tắt so sánh 3 card

| Card | Bằng chứng nguồn | Metric | Mức phù hợp với LLM | Rủi ro chính |
|------|------------------|--------|---------------------|--------------|
| 1. VinFast cảnh báo sớm lỗi | Mạnh (tài liệu NHTSA có mốc ngày) | Có baseline 11 / 15 ngày | Cao (gom văn bản tự do) | Cảnh báo sai; thiếu dữ liệu gắn nhãn |
| 2. Vinhomes theo dõi SLA | Mạnh (quy định chính thức) | Có ngưỡng SLA, chưa có baseline | Thấp–trung bình (rule là chính) | Rule-based có thể đủ |
| 3. VinFast phụ tùng | Trung bình (workflow là suy luận) | Có ngưỡng 24 giờ, chưa có baseline | Thấp (bài toán thống kê) | Có thể NO-GO cho LLM |

---

## Nguồn

- N1: NHTSA, báo cáo triệu hồi 23V-355 — https://static.nhtsa.gov/odi/rcl/2023/RCLRPT-23V355-3195.PDF
- N2: NHTSA, điều tra PE24025 — https://static.nhtsa.gov/odi/inv/2024/INOA-PE24025-19754.pdf
- N3: VinFast VF 8 reliability and recalls (tổng hợp dữ liệu NHTSA, recall 25V-559) — https://thechargeport.com/ev/vinfast-vf-8/reliability
- N4: Quy định xử lý khiếu nại của Vinhomes — https://market-files.vinhomes.vn/public-mngt/d/279c2b19-3e4d-406a-950b-be78102f50fa
- N5: InsideEVs, chính sách hậu mãi đặc biệt của VinFast (06/2023) — https://insideevs.com/news/672154/vinfast-cash-for-vehicle-breakdowns-vf8/ ; TTAC — https://www.thetruthaboutcars.com/cars/news-blog/vinfast-to-pay-customers-who-need-extended-repairs-44503828
- N6: Vietnam News, hậu mãi VinFast, phụ tùng 24 giờ — https://bizhub.vietnamnews.vn/vinfast-announces-incentives-for-its-aftersales-service-post360275.html
- N7: Just Auto, doanh số quốc tế VinFast 2025 — https://www.just-auto.com/newsletters/vinfasts-global-deliveries-rise-102-in-2025
- N8: Green SM, chương trình sạc tại nhà — https://www.greensm.com/vn-vi/news/sac-xanh-tai-nha-2025
- N9: Vietnam News, hạ tầng sạc Hà Nội — https://vietnamnews.vn/society/1722437/ha-noi-faces-urgent-need-for-ev-charging-infrastructure-ahead-of-fossil-fuel-motorbike-phase-out.html
- N10: Index.vn, V-Green và PVOIL — https://index.vn/en/news/pvoil-to-install-nearly-3000-battery-swapping-cabinets-for-electric-motorcycles-and-vinfast-charging-stations-across-its-nationwide-network
- N11: Báo Pháp Luật, trạm sạc V-Green Đông Anh — https://doanhnhan.baophapluat.vn/dong-anh-co-tram-sac-v-green-ky-luc-voi-gan-200-cong-sac-o-to-1000-cong-sac-xe-may-dien-86696.html
- N12: Visor.ph, chính sách pin 70% của VinFast — https://visor.ph/?p=238944
- N13: Sổ bảo hành VinFast VF 9 (Canada) — https://static-cms-prod.vinfastauto.ca/statics/2025-07/2025-vinfast-canada-vf-9-warranty-booklet-1.2-livret-de-garantie-1.2.pdf
- N14: VietnamNet, doanh số VinFast 2026 — https://vietnamnet.vn/en/charging-infrastructure-key-to-vietnam-s-electric-mobility-transition-2523619.html
