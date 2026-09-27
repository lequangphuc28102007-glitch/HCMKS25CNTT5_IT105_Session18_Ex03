# PHẢN BIỆN TOÀN DIỆN BẢN THẢO SRS RIKKEITRAVEL
## Phân hệ: Đặt tour trọn gói

---

## Bước 1 — Bảng lỗi phát hiện được

| STT | Nội dung lỗi | Phân loại lỗi (Sai vị trí IEEE 830 / Vi phạm đặc tính vàng nào) | Vị trí đúng hoặc cách khắc phục |
|-----|--------------|-----------------------------------------------------------------|---------------------------------|
| 1 | Danh sách Actor và Use Case được đặt trong **2.4 Constraints** | **Sai vị trí IEEE 830** | Chuyển sang **2.2 Product Functions** (hoặc 2.3 User Characteristics). Constraints chỉ chứa ràng buộc thiết kế, pháp lý, kỹ thuật – không chứa Actor/Use Case. |
| 2 | Sơ đồ ERD được đặt trong **1.3 Definitions** | **Sai vị trí IEEE 830** | Chuyển sang **3.4 Logical Database Requirements** (hoặc Appendix Supporting Information). 1.3 chỉ dành cho định nghĩa thuật ngữ, viết tắt. |
| 3 | REQ-21: “phản hồi báo giá … một cách nhanh chóng” | **Vi phạm Verifiable (Có thể kiểm chứng)** | Viết lại với chỉ số đo lường cụ thể (thời gian phản hồi, điều kiện tải). |
| 4 | REQ-23 và REQ-24 mâu thuẫn trực tiếp về quyền chỉnh sửa báo giá sau khi khách hàng đã xác nhận thanh toán | **Vi phạm Consistent (Nhất quán)** | Xóa một trong hai, hoặc viết lại thành **một quy tắc nghiệp vụ duy nhất** thống nhất toàn tài liệu. |
| 5 | REQ-25: “có thể được cân nhắc bổ sung sau, tùy nguồn lực” | **Vi phạm Ranked for Importance / Stability** (và một phần Necessary) | Bổ sung mức độ ưu tiên rõ ràng (Must / Should / Could / Won’t – MoSCoW) và quyết định dứt điểm có đưa vào phạm vi hay không. |
| 6 | REQ-22 chỉ mô tả trường hợp thanh toán đủ 100%, thiếu xử lý thanh toán thiếu / thừa / thất bại | **Vi phạm Complete (Đầy đủ)** | Bổ sung các kịch bản thanh toán không hợp lệ hoặc không đủ điều kiện. |
| 7 | Phạm vi (1.2 Scope) chỉ nêu “nhóm từ 10 người trở lên” nhưng không làm rõ giới hạn trên, loại tour, hoặc đối tượng không được hỗ trợ | **Vi phạm Complete / Unambiguous** (phạm vi) | Bổ sung làm rõ phạm vi (in-scope / out-of-scope) rõ ràng hơn trong 1.2 Scope. |

**Tóm tắt nhóm lỗi đã phát hiện:**
- Sai cấu trúc IEEE 830: 2 lỗi (Actor/Use Case + ERD).
- Vi phạm đặc tính vàng: Verifiable, Consistent, Ranked for Importance, Complete (đạt yêu cầu tối thiểu 3 đặc tính khác nhau).

---

## Bước 2 — Bản chỉnh sửa hoàn chỉnh

### 1. Viết lại các mục bị sai vị trí

**1.2 Scope** (giữ nguyên + bổ sung làm rõ):
```
Hệ thống hỗ trợ khách hàng doanh nghiệp đặt tour trọn gói cho nhóm từ 10 người trở lên.
In-scope: Tạo yêu cầu đặt tour, nhận báo giá, xác nhận thanh toán, quản lý trạng thái đơn hàng.
Out-of-scope: Đặt tour cá nhân (< 10 người), thanh toán trả góp nhiều kỳ, tích hợp hệ thống kế toán của khách hàng.
```

**2.2 Product Functions** (chuyển Actor & Use Case từ 2.4 Constraints sang đây):
```
Actor chính:
- Nhân viên đặt tour (phía khách hàng doanh nghiệp)
- Điều phối viên RikkeiTravel

Use Case chính:
- Tạo yêu cầu đặt tour
- Duyệt báo giá
- Xác nhận thanh toán
```

**2.4 Constraints** (để trống hoặc chỉ chứa ràng buộc thực sự, ví dụ):
```
- Hệ thống phải tuân thủ quy định bảo vệ dữ liệu cá nhân theo pháp luật Việt Nam.
- Giao diện phải hỗ trợ tiếng Việt làm ngôn ngữ mặc định.
```

**1.3 Definitions, Acronyms, and Abbreviations** (xóa ERD, chỉ giữ định nghĩa thuật ngữ nếu có):
```
- Tour trọn gói: Gói dịch vụ du lịch bao gồm đầy đủ các hạng mục đã thỏa thuận (vé máy bay, khách sạn, ăn uống, hướng dẫn viên…).
- Báo giá: Văn bản đề xuất chi phí và điều kiện dịch vụ do Điều phối viên RikkeiTravel gửi cho khách hàng doanh nghiệp.
```

**3.4 Logical Database Requirements** (chuyển ERD sang đây):
```
Sơ đồ ERD lưu trữ quan hệ giữa các thực thể: Yêu cầu đặt tour – Báo giá – Hợp đồng.
(Đính kèm sơ đồ ERD tại đây hoặc trong Appendix)
```

### 2. Viết lại REQ-21 đạt chuẩn Verifiable

**REQ-21 (viết lại):**
Sau khi hệ thống nhận yêu cầu đặt tour hợp lệ từ Nhân viên đặt tour, hệ thống phải tạo và gửi báo giá sơ bộ đến khách hàng doanh nghiệp trong thời gian ≤ 30 phút (tính từ lúc yêu cầu được ghi nhận thành công) với tỷ lệ thành công ≥ 98% trong điều kiện tải bình thường (≤ 50 yêu cầu đồng thời).

### 3. Xử lý dứt điểm mâu thuẫn giữa REQ-23 và REQ-24

**Quy tắc nghiệp vụ duy nhất được thống nhất:**

**REQ-23/24 (thay thế cả hai bằng một yêu cầu nhất quán):**
Sau khi khách hàng doanh nghiệp xác nhận thanh toán đủ 100% giá trị hợp đồng và hệ thống chuyển trạng thái đơn sang “Đã xác nhận”:
- Mọi thông tin trên báo giá và hợp đồng được **khóa** (read-only).
- Không ai (bao gồm Điều phối viên) được chỉnh sửa nội dung báo giá/hợp đồng đã khóa.
- Nếu cần điều chỉnh, Điều phối viên phải tạo **báo giá bổ sung / phụ lục hợp đồng** mới và gửi lại cho khách hàng phê duyệt.

(REQ-23 cũ bị loại bỏ hoàn toàn vì mâu thuẫn với quy tắc khóa sau xác nhận.)

### 4. Bổ sung mức độ ưu tiên rõ ràng cho REQ-25

**REQ-25 (viết lại + xếp hạng):**
Hệ thống **nên** (Should – mức độ ưu tiên trung bình) cung cấp tính năng xuất báo giá ra file PDF.
- Ưu tiên: Should (MoSCoW)
- Lý do: Tăng tiện lợi cho khách hàng doanh nghiệp lưu trữ và trình ký nội bộ, nhưng không ảnh hưởng đến luồng nghiệp vụ cốt lõi.
- Nếu nguồn lực hạn chế trong giai đoạn 1, có thể chuyển sang giai đoạn 2 (ghi rõ trong Apportioning of Requirements).

### 5. Bổ sung REQ-22 cho đầy đủ (Complete)

**REQ-22 (viết lại đầy đủ):**
Khi khách hàng xác nhận thanh toán:
1. Nếu thanh toán đủ 100% giá trị hợp đồng → Hệ thống tự động chuyển trạng thái đơn sang “Đã xác nhận” và khóa báo giá/hợp đồng.
2. Nếu thanh toán thiếu → Hệ thống giữ nguyên trạng thái “Chờ thanh toán”, hiển thị số tiền còn thiếu và gửi thông báo nhắc nhở.
3. Nếu thanh toán thất bại hoặc bị hủy → Hệ thống ghi nhận trạng thái “Thanh toán thất bại”, cho phép khách hàng thử lại hoặc liên hệ Điều phối viên.

---

**Tóm tắt thay đổi chính:**
- Đã chuyển Actor/Use Case và ERD về đúng vị trí IEEE 830.
- Đã khắc phục 4 đặc tính vàng: Verifiable (REQ-21), Consistent (REQ-23/24), Ranked (REQ-25), Complete (REQ-22 + Scope).
- Bản thảo sau chỉnh sửa sẵn sàng trình khách hàng ký duyệt.