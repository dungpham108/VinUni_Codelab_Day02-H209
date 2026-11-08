# 01 — Problem Scan: Vinpearl Group Booking Intake

**Tên nhóm:** [Điền tên nhóm]  
**Thành viên:** [Điền họ tên và email]  
**Người phụ trách:** [Điền họ tên]

> **Quy ước bằng chứng:** Đây là đề xuất scoping cho bài lab, không phải mô tả đã được xác nhận về hệ thống nội bộ Vinpearl. Những bước vận hành chưa có tài liệu xác minh được đánh dấu **[Giả thuyết]**. Mọi số liệu minh họa được đánh dấu **[Giả định]**; không trình bày chúng như số liệu thực tế của Vinpearl.

---

## Phase 1 — SCAN

### Bài toán được chọn

| Công ty | Lens | Bài toán | Vì sao đáng khảo sát |
|---|---|---|---|
| Vinpearl | Repetitive · Time-consuming · AI-upgrade · Stakeholder Pain | Nhân viên kinh doanh/đặt phòng phải đọc email đặt phòng đoàn, tự trích xuất thông tin và nhập lại vào hệ thống trước khi kiểm tra khả năng đáp ứng phòng. | Email của doanh nghiệp/lữ hành có thể chứa nhiều ngày lưu trú, loại phòng, số khách, yêu cầu ăn uống và điều kiện thanh toán trong văn bản tự do. Thiếu trường hoặc hiểu sai thông tin có thể tạo ra báo giá/đề nghị đặt phòng sai. |

### Phạm vi bài toán

- **Đầu vào:** Email yêu cầu đặt phòng đoàn và các tệp đính kèm do đối tác gửi.
- **Đầu ra mong muốn:** Bản nháp yêu cầu đặt phòng có cấu trúc, kèm trích dẫn đoạn văn bản nguồn và danh sách trường còn thiếu.
- **Không nằm trong phạm vi:** Xác nhận booking, giữ phòng, báo giá cuối cùng, thu tiền hoặc cam kết dịch vụ với khách.
- **Người chịu trách nhiệm:** Nhân viên đặt phòng/kinh doanh đoàn xác minh và gửi yêu cầu kiểm tra tồn phòng theo quy trình hiện hành.

### Mức độ bằng chứng

| Nội dung | Trạng thái |
|---|---|
| Vinpearl kinh doanh dịch vụ lưu trú và du lịch | Bối cảnh doanh nghiệp trong đề bài |
| Đối tác có thể gửi yêu cầu đoàn qua email | **[Giả thuyết nghiệp vụ]**, cần phỏng vấn nhân viên |
| Nhân viên phải nhập lại dữ liệu từ email vào hệ thống | **[Giả thuyết]**, cần quan sát workflow |
| Thời gian xử lý trung bình hiện tại | **[Chưa có dữ liệu]** |
| Tỷ lệ sai/thiếu thông tin trong yêu cầu đoàn | **[Chưa có dữ liệu]** |
| Khối lượng email booking đoàn mỗi ngày | **[Chưa có dữ liệu]** |

### Phase 2 — Quick Problem Card

| Trường | Nội dung |
|---|---|
| Bài toán | Chuẩn hóa email yêu cầu đặt phòng đoàn thành bản nháp booking có cấu trúc để giảm thao tác đọc và nhập liệu thủ công. |
| Actor | Nhân viên đặt phòng đoàn / nhân viên kinh doanh phụ trách đối tác. |
| Workflow hiện tại | **[Giả thuyết]** (1) Nhận email và file đính kèm. (2) Đọc nội dung, tìm ngày đến/đi, số khách, số phòng và loại phòng. (3) Hỏi lại đối tác nếu thiếu hoặc mâu thuẫn. (4) Nhập thông tin vào hệ thống nội bộ. (5) Chuyển bộ phận liên quan kiểm tra tồn phòng và điều kiện giá. |
| Bottleneck cần đo | Đọc email và chuyển dữ liệu tự do thành các trường có cấu trúc; đặc biệt khi email có nhiều hạng mục hoặc thông tin không nhất quán. |
| AI hỗ trợ | Trích xuất trường dữ liệu, chuẩn hóa định dạng ngày/số lượng, phát hiện thiếu hoặc mâu thuẫn, tạo bản nháp và chỉ rõ bằng chứng nguồn. |
| Metric đề xuất | **[Giả định mục tiêu thử nghiệm]** giảm median thời gian tạo bản nháp từ baseline đo thực tế xuống ít nhất 40%; độ chính xác trường bắt buộc ≥ 98% trên tập kiểm thử đã gán nhãn. |
| Quick architecture | LLM feature + validation bằng rule. Không dùng agent tự gửi email hoặc tạo booking. |

### Giả thuyết cần kiểm chứng trước khi triển khai

1. Email booking đoàn là một nguồn đầu vào đủ phổ biến để tạo ra lợi ích đáng kể.
2. Nhân viên đang nhập lại một phần thông tin đã có trong email.
3. Có thể định nghĩa schema booking đoàn ổn định và thống nhất giữa các khách sạn.
4. Dữ liệu email có thể được xử lý theo chính sách bảo mật và phân quyền của doanh nghiệp.

**Kết luận sơ bộ:** Đưa vào Deep-Dive ở mức **NOT YET** cho đến khi quan sát workflow, đo baseline và xác nhận quyền truy cập dữ liệu. Có thể làm prototype trên dữ liệu giả lập hoặc đã ẩn danh.
