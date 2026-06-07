# 🔒 Security Policy

## Phiên Bản Được Hỗ Trợ (Supported Versions)

Bảng dưới đây cho biết các phiên bản hiện đang được hỗ trợ cập nhật bảo mật:

| Phiên bản | Hỗ trợ             | Ghi chú                        |
| --------- | ------------------- | ------------------------------ |
| 1.0.x     | :white_check_mark:  | Phiên bản hiện tại - ổn định   |
| < 1.0     | :x:                 | Không còn hỗ trợ               |

> **Lưu ý:** Dự án đang ở phiên bản **1.0.x**. Bảng sẽ được cập nhật khi có các phiên bản mới.

---

## 🛡️ Kiến Trúc Bảo Mật

### Mô Hình Ứng Dụng

Ứng dụng **Chấm Công Nhân Viên SW SOC** là một web app tĩnh (static site) chạy hoàn toàn trên phía client (trình duyệt). Dưới đây là các đặc điểm bảo mật chính:

| Thành phần            | Mô tả                                                                 |
| --------------------- | --------------------------------------------------------------------- |
| **Hosting**           | GitHub Pages - HTTPS mặc định, không có server-side code              |
| **Dữ liệu nguồn**    | Google Sheets (chỉ đọc qua CSV export công khai)                      |
| **Xác thực Admin**    | Mật khẩu băm SHA-256 lưu trên `localStorage` của trình duyệt         |
| **Phiên đăng nhập**   | `sessionStorage` - tự hết hạn khi đóng trình duyệt                   |
| **Lưu trữ cấu hình** | `localStorage` - chỉ lưu trên thiết bị của người dùng                 |
| **Truyền tải dữ liệu**| HTTPS (TLS) cho mọi request tới Google Sheets và GitHub Pages         |

### Không Thu Thập Dữ Liệu Cá Nhân

- ❌ Không có backend server hay cơ sở dữ liệu
- ❌ Không thu thập, lưu trữ hay gửi dữ liệu cá nhân về bất kỳ server nào
- ❌ Không sử dụng cookies theo dõi
- ❌ Không tích hợp công cụ analytics hay tracking
- ✅ Toàn bộ dữ liệu xử lý trên trình duyệt người dùng (client-side only)

---

## ⚠️ Báo Cáo Lỗ Hổng Bảo Mật (Reporting a Vulnerability)

Nếu bạn phát hiện lỗ hổng bảo mật, **vui lòng KHÔNG tạo Issue công khai**. Thay vào đó, hãy liên hệ trực tiếp:

### Kênh Báo Cáo

| Kênh         | Thông tin                              |
| ------------ | -------------------------------------- |
| 📧 **Email** | tainguyenhr.dev@gmail.com              |
| 📱 **Zalo**  | 0586482344                             |

### Quy Trình Xử Lý

1. **Gửi báo cáo** → Mô tả chi tiết lỗ hổng, kèm các bước tái hiện (nếu có)
2. **Xác nhận** → Chúng tôi sẽ phản hồi trong vòng **48 giờ**
3. **Đánh giá** → Xác minh và đánh giá mức độ nghiêm trọng trong **7 ngày**
4. **Khắc phục** → Phát hành bản vá trong thời gian sớm nhất
5. **Thông báo** → Cập nhật kết quả cho người báo cáo

### Mức Độ Nghiêm Trọng

| Mức độ       | Thời gian phản hồi | Ví dụ                                   |
| ------------ | ------------------- | ---------------------------------------- |
| 🔴 Nghiêm trọng | 24 giờ           | Lộ dữ liệu nhân viên, XSS nguy hiểm    |
| 🟠 Cao       | 48 giờ              | Bypass xác thực admin                    |
| 🟡 Trung bình| 7 ngày              | Lỗi logic, CSRF                         |
| 🟢 Thấp      | 14 ngày             | UI bug liên quan bảo mật, lỗi hiển thị  |

---

## 🔍 Kiểm Định An Toàn Mã Nguồn

### Tiêu Chuẩn Tuân Thủ

Dự án được phát triển và kiểm định theo các tiêu chuẩn an toàn cộng đồng:

#### ✅ OWASP Top 10 (Web Application)

| Mối đe dọa                    | Trạng thái         | Biện pháp                                         |
| ------------------------------ | ------------------- | ------------------------------------------------- |
| A01 - Broken Access Control    | ✅ Đã xử lý        | Phân quyền Admin/Nhân viên, trang admin riêng biệt |
| A02 - Cryptographic Failures   | ✅ Đã xử lý        | SHA-256 hash mật khẩu, HTTPS truyền tải           |
| A03 - Injection                | ✅ Đã xử lý        | Không có SQL/NoSQL, dữ liệu chỉ đọc CSV           |
| A05 - Security Misconfiguration| ✅ Đã xử lý        | Không expose API keys hay credentials              |
| A07 - XSS                      | ✅ Đã xử lý        | Sử dụng `textContent` thay vì `innerHTML` khi render dữ liệu người dùng |
| A09 - Security Logging         | ⚠️ Giới hạn        | Client-side app, không có server logging           |

#### ✅ Bảo Mật Client-Side

- [x] Không lưu mật khẩu dạng plaintext (`SHA-256` hash)
- [x] Phiên đăng nhập admin tự hết hạn (`sessionStorage`)
- [x] Trang admin tách biệt khỏi trang nhân viên
- [x] Không sử dụng `eval()` hay `Function()` constructor
- [x] Input validation cho mã CTV và URL Google Sheets
- [x] CSP-friendly: không sử dụng inline scripts trong HTML

#### ✅ Bảo Mật Dữ Liệu

- [x] Dữ liệu Google Sheets chỉ truy cập dạng **read-only** (CSV export)
- [x] Không lưu trữ dữ liệu nhân viên trên server
- [x] Dữ liệu cache trong bộ nhớ (`state`) tự xóa khi đóng tab
- [x] `localStorage` chỉ lưu: URL sheet, theme preference, mã tìm kiếm gần đây

---

## 🏗️ Cấu Trúc Mã Nguồn

```
chamcongnhanvien.swsoc/
├── index.html          # Trang nhân viên (công khai)
├── admin.html          # Trang quản trị (yêu cầu đăng nhập)
├── app.js              # Logic xử lý chính
├── style.css           # Giao diện responsive
├── SECURITY.md         # Chính sách bảo mật (file này)
└── README.md           # Hướng dẫn sử dụng
```

### Phân Quyền

| Trang        | Đối tượng    | Quyền                                                |
| ------------ | ------------ | ---------------------------------------------------- |
| `index.html` | Nhân viên    | Chỉ tra cứu chấm công bằng mã CTV                   |
| `admin.html` | Quản trị viên| Cấu hình Google Sheets, đăng thông báo, đổi mật khẩu |

---

## 📋 Lưu Ý Bảo Mật Cho Quản Trị Viên

1. **Đổi mật khẩu mặc định** ngay sau lần đăng nhập đầu tiên
2. **Không chia sẻ** đường dẫn `admin.html` cho nhân viên
3. **Publish Google Sheets** ở chế độ chỉ đọc (read-only)
4. **Không đặt thông tin nhạy cảm** (CMND, tài khoản ngân hàng...) trong Google Sheets
5. **Kiểm tra định kỳ** quyền truy cập Google Sheets

---

## 📜 Giấy Phép & Trách Nhiệm

- Mã nguồn được cung cấp "nguyên trạng" (as-is)
- Người sử dụng chịu trách nhiệm bảo mật dữ liệu Google Sheets của mình
- Khuyến khích cộng đồng đóng góp cải thiện bảo mật qua Pull Request

---

## 📞 Liên Hệ

| Thông tin     | Chi tiết                     |
| ------------- | ---------------------------- |
| 👤 Tác giả    | Tài Nguyễn                   |
| 📧 Email      | tainguyenhr.dev@gmail.com    |
| 📱 Zalo       | 0586482344                   |

---

> *Cập nhật lần cuối: 08/06/2026*
