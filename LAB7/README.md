 Lab 7: Xây dựng hệ thống Lọc Web & Proxy An toàn (Squid, E2Guardian kết hợp pfSense)

## Mục tiêu bài Lab
1. **Thiết lập Web Proxy:** Sử dụng máy chủ Ubuntu làm trạm trung chuyển kiểm soát toàn bộ lưu lượng web.
2. **Lọc nội dung thông minh:** Chặn các trang web không phù hợp hoặc nằm trong danh sách đen (Blacklist) bằng **E2Guardian**.
3. **Chống vượt rào (Anti-Bypass):** Cấu hình tường lửa **pfSense** để ép buộc mọi máy tính trong mạng phải đi qua Proxy, tuyệt đối không cho phép người dùng tự ý tắt Proxy để đi thẳng ra ngoài Internet.

## Mô hình & Luồng hoạt động
* **pfSense Firewall (`10.6.0.1`):** Đóng vai trò gác cửa. Chặn đứng mọi kết nối đi web trực tiếp từ máy trạm, đồng thời điều hướng các yêu cầu hợp lệ chạy thẳng vào cổng `8080` của máy Proxy.
* **Ubuntu Proxy Server (`10.6.0.30`):** 
  * **E2Guardian (Cổng `8080`):** Kiểm tra chi tiết nội dung trang web (Layer 7), đối chiếu danh sách chặn (`bannedsitelist`). Nếu phát hiện trang bị cấm, hệ thống sẽ trả về màn hình thông báo *Access Denied*.
  * **Squid (Cổng `3128`):** Nhận yêu cầu đã qua kiểm duyệt từ E2Guardian để tải dữ liệu thực tế từ Internet về cho người dùng.
## Các bước thực hiện chính
1. **Cấu hình trên pfSense:** Tạo các nhóm địa chỉ (Aliases) cho client và proxy, sau đó thiết lập luật tường lửa (Firewall Rules) ở tab LAN để chặn kết nối trực tiếp và chuyển hướng lưu lượng.
2. **Cài đặt & Khởi chạy dịch vụ trên Ubuntu:** Kích hoạt Squid và E2Guardian, kiểm tra các cổng dịch vụ ở trạng thái `LISTEN`.
3. **Kiểm thử kết quả:** 
   * Truy cập thử trang web nằm trong danh sách đen $\rightarrow$ Bị chặn thành công với thông báo *Access Denied* và ghi log hệ thống (`*DENIED*`).
   * Truy cập trang web hợp lệ $\rightarrow$ Tải trang bình thường (`HTTP/1.1 200 OK`).
4. **Bảo mật & Toàn vẹn:** Trích xuất file nhật ký (log) và tạo mã băm `SHA-256` để kiểm chứng toàn vẹn dữ liệu nộp báo cáo.

##  Những điểm cần lưu ý khi làm bài
* **Thứ tự Luật trên pfSense:** Các luật chặn (Block) và chuyển hướng (Pass) phải nằm ở vị trí trên cùng của bảng LAN Rules thì mới có tác dụng ép buộc. Sau khi sửa luật, bắt buộc phải thực hiện *Reset States* để xóa sạch các kết nối cũ đang tồn đọng.
* **Đường dẫn file cấu hình:** Thư mục chứa danh sách chặn của E2Guardian có thể khác nhau tùy phiên bản hệ điều hành Ubuntu, nên dùng ký tự đại diện khi cần thao tác nhanh.
* **Phân quyền Log:** Các file nhật ký hệ thống do quyền `root` quản lý, nhớ cấp quyền đọc (`chmod`) trước khi thực hiện xuất mã băm SHA-256.
