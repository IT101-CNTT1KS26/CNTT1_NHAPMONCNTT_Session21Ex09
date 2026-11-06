# XỬ LÝ SỰ CỐ TRỤ SẠC VOLTCHARGE #04 — BẾN XE MIỀN ĐÔNG


## PHẦN 1: THIẾT KẾ PROMPT 3 THÀNH PHẦN CHUẨN HÓA

### 1. Phân tích nguyên nhân câu hỏi cụt ngủn dẫn đến hiện tượng AI Hallucination (Ảo giác)

* **Thiếu ngữ cảnh (Missing Context):** Câu hỏi *"AI ơi phần mềm trạm sạc của tôi bị hỏng rồi làm sao đây?"* hoàn toàn trống rỗng về thông tin kỹ thuật (không rõ hệ điều hành gì, phần mềm nào, thông báo lỗi ra sao).
* **Cơ chế dự đoán của AI:** Do không có dữ liệu đầu vào cụ thể, mô hình ngôn ngữ lớn (LLM) buộc phải sử dụng xác suất thống kê để "đoán mò" (hallucination) dựa trên các từ khóa chung chung như "phần mềm trạm sạc", "hỏng".
* **Hậu quả:** AI đưa ra các phỏng đoán sai lệch nghiêm trọng (khuyên tháo tung vỏ sắt trạm sạc kiểm tra dây điện vật lý), trong khi bản chất vấn đề nằm ở **quyền truy cập phần mềm hệ thống**.

---

### 2. Soạn thảo câu Prompt 3 Thành phần hoàn chỉnh

Dựa trên công thức chuẩn kỹ sư, câu Prompt được cấu trúc rõ ràng thành 3 phần:

```text
[Context]
Tôi là kỹ sư vận hành hệ thống Kiosk nạp năng lượng thông minh VoltCharge chạy trên hệ điều hành Windows 11 tại Bến xe Miền Đông. Trụ sạc nhanh số 04 đang gặp sự cố phần mềm khi khởi động dịch vụ ControllerDaemon.

[Raw Error]
Dưới đây là nguyên văn nhật ký lỗi từ màn hình Terminal:
2026-09-21 21:15:30 [FATAL] ControllerDaemon failed to start.
System.UnauthorizedAccessException: Access to the path 'C:\Program Files\VoltCharge\data\session.log' is denied.
  at System.IO.FileStream.ValidateFileHandle(SafeFileHandle handle)
  at VoltCharge.Core.SessionManager.Initialize()
InnerException: None. Process terminated with exit code 5 (0x00000005).
Suggestion: Run the application with elevated administrator privileges or verify NTFS file permissions.

[Instruction]
Hãy đóng vai trò là chuyên gia hỗ trợ kỹ thuật:
1. Giải thích nguyên nhân gốc rễ của lỗi trên bằng tiếng Việt bình dân, dễ hiểu cho kỹ thuật viên mới.
2. Đưa ra chính xác 3 bước xử lý cụ thể tại chỗ trên Windows 11 để khắc phục sự cố này.

```

---

## PHẦN 2: GIẢI MÃ LỖI & KỊCH BẢN KHẮC PHỤC

### 3. Giải mã lỗi `UnauthorizedAccessException` bằng tiếng Việt đời thường

* **Bản chất lỗi:** Ứng dụng quản lý trạm sạc (`ControllerDaemon`) đang cố gắng ghi nhật ký phiên làm việc vào tệp `session.log` nằm bên trong thư mục hệ thống `C:\Program Files\VoltCharge\data\`.
* **Nguyên nhân:** Windows 11 bảo vệ nghiêm ngặt thư mục `Program Files`. Ứng dụng đang chạy ở tài khoản người dùng thông thường (Standard User) nên **không có quyền ghi dữ liệu** vào khu vực này, dẫn đến việc bị từ chối truy cập (Access is denied) và sập ứng dụng với mã lỗi `exit code 5`.

---

### 4. 3 bước xử lý tại chỗ trên Windows 11 để khôi phục trạm sạc

1. **Bước 1: Chạy ứng dụng với quyền Quản trị viên:** Kích hoạt quyền Elevated Administrator.
Thoát hẳn ứng dụng hiện tại. Tìm đến biểu tượng khởi chạy phần mềm VoltCharge (hoặc file thực thi của ControllerDaemon), nhấp chuột phải và chọn **Run as administrator**. Thao tác này cấp quyền cao nhất cho phép ứng dụng ghi file vào thư mục hệ thống.


2. **Bước 2: Phân quyền NTFS cho thư mục Data:** Thay đổi quyền ghi vĩnh viễn trên Windows 11.
Nhấp chuột phải vào thư mục chứa dữ liệu `C:\Program Files\VoltCharge\data` > Chọn **Properties** > Chuyển sang thẻ **Security** > Nhấn nút **Edit**. Chọn tài khoản người dùng đang vận hành trạm và tích chọn vào ô **Full Control** (hoặc **Modify**) ở cột Allow > Nhấn **Apply** và **OK** để lưu lại.


3. **Bước 3: Khởi động lại dịch vụ và kiểm tra:** Xác thực hệ thống hoạt động ổn định.
Khởi động lại phần mềm Kiosk hoặc restart lại dịch vụ ControllerDaemon trên Windows. Quan sát màn hình Terminal để đảm bảo dòng lệnh không còn báo lỗi `UnauthorizedAccessException` và trạm sạc đã cấp điện bình thường cho xe.


---

### 5. Xử lý bẫy an ninh: Chuỗi chứa mã bí mật API_KEY

* **Hành động bắt buộc:** Kỹ sư **tuyệt đối không được** sao chép nguyên văn chuỗi chứa khóa bí mật (`volt_secret_token_889922`) dán lên khung chat của ChatGPT hay Gemini. Phải thực hiện thao tác **che mờ, xóa bỏ hoặc thay thế** chuỗi này bằng một giá trị giả lập an toàn (ví dụ: `API_KEY=YOUR_SECRET_KEY_HERE`) trước khi gửi Prompt.
* **Giải thích lý do:**
1. **Bảo mật dữ liệu doanh nghiệp:** Khóa API (API_KEY) là chìa khóa tối cao cho phép truy cập vào hệ thống backend quản lý trạm sạc. Gửi lên AI công cộng đồng nghĩa với việc làm rò rỉ thông tin xác thực doanh nghiệp.
2. **Quy tắc An toàn Vàng:** Các mô hình AI công cộng có thể lưu trữ lịch sử hội thoại hoặc sử dụng dữ liệu đầu vào để huấn luyện mô hình, tạo ra rủi ro nghiêm trọng bị kẻ xấu khai thác từ cơ sở dữ liệu của nhà cung cấp AI.