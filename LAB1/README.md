# Báo cáo Bài thực hành: An Toàn Hệ Thống Thông Tin (LAB_AT_BMHTTT)
**Chủ đề:** Khảo sát, so sánh Telnet và SSH, triển khai xác thực và bảo mật hệ thống từ xa.


## 1. Giới thiệu chung
Bài lab này thực hiện việc tìm hiểu, so sánh và đánh giá mức độ bảo mật giữa hai giao thức quản trị từ xa truyền thống **Telnet** và hiện đại **SSH**. Đồng thời thực hành cấu hình các cơ chế xác thực an toàn trên hệ điều hành Ubuntu.

## 2. Nội dung và Trả lời câu hỏi lý thuyết

### **Câu 1: Telnet và SSH là gì và được ứng dụng trong trường hợp nào?**
* **Telnet:** Giao thức mạng chuẩn trên nền TCP/IP cho phép điều khiển từ xa qua dòng lệnh nhưng truyền dữ liệu dạng văn bản rõ (plaintext), bảo mật rất thấp.
* **SSH (Secure Shell):** Giao thức quản trị từ xa an toàn thay thế Telnet với toàn bộ dữ liệu được mã hóa mạnh mẽ.
* **Ứng dụng:** Quản trị hệ thống máy chủ Linux/VPS, cấu hình thiết bị mạng từ xa, kết hợp làm nền tảng cho SFTP/SCP.

### **Câu 2: So sánh Telnet và SSH**
| Tiêu chí | Telnet | SSH |
| :--- | :--- | :--- |
| **Cổng mạng (Port)** | Cổng 23 | Cổng 22 |
| **Mã hóa** | Không mã hóa (Plaintext) | Mã hóa bảo mật cao (AES,...) |
| **Bảo mật** | Rất thấp (dễ bị bắt gói tin đọc pass) | Rất cao (chống nghe lén, giả mạo) |
| **Xác thực** | Username/Password gửi dạng rõ | Hỗ trợ Username/Password và Public-Key |

### **Câu 3: Xác thực bằng khóa công khai (Public-key authentication)**
* Sử dụng cặp khóa bất đối xứng (`Private Key` giữ bí mật trên client, `Public Key` đưa lên server).
* Giúp đăng nhập nhanh chóng, an toàn tuyệt đối mà không cần truyền mật khẩu qua mạng.

### **Câu 4: Phân tích bảo mật (Mô hình CIA)**
* **Tính bí mật (Confidentiality):** Telnet lộ toàn bộ dữ liệu; SSH mã hóa toàn bộ payload.
* **Tính toàn vẹn (Integrity):** Telnet dễ bị chèn/sửa lệnh; SSH sử dụng mã MAC kiểm soát chống giả mạo.
* **Tính xác thực (Authentication):** Telnet yếu (gửi pass thô); SSH mạnh với cặp khóa và xác thực máy chủ.

### **Câu 5: Phân tích gói tin qua Wireshark**
* **Telnet:** Quan sát được tường tận username, password và các lệnh thao tác dạng chữ rõ.
* **SSH:** Nội dung bên trong hoàn toàn bị mã hóa thành các ký tự rác (ciphertext), bảo vệ thông tin an toàn.

### **Câu 6: Tại sao mật khẩu phức tạp không cứu được Telnet?**
* Dù mật khẩu có dài và phức tạp đến đâu, do Telnet hoàn toàn không mã hóa đường truyền, mọi ký tự vẫn bay trần trụi trên mạng và bị bắt trọn vẹn bằng Wireshark.

### **Câu 7: Metadata quan sát được trên SSH**
* Mặc dù nội dung mã hóa, Wireshark vẫn thấy được IP nguồn/đích, cổng 22, kích thước gói tin và thời gian truyền. Rủi ro tiềm ẩn là lộ phiên bản phần mềm hoặc bị phân tích lưu lượng (Traffic Analysis).

### **Câu 8: Vai trò của Host Key trong SSH**
* Xác thực định danh máy chủ, đảm bảo kết nối đúng server đích chứ không phải kẻ giả mạo (Man-in-the-Middle). Chấp nhận mù quáng dễ dẫn đến nguy cơ bị đánh cắp dữ liệu.

### **Câu 9: Lưu lượng Unicast trong mạng LAN**
* Trong mạng hiện đại dùng Switch, các gói tin unicast đi trực tiếp qua các cổng tương ứng chứ không quảng bá như Hub cũ. Để bắt được lưu lượng này, kẻ tấn công cần dùng kỹ thuật **ARP Spoofing** hoặc **Port Mirroring (SPAN)**.

### **Câu 10: Nguyên lý Public-key Authentication**
* Server tạo thử thách mã hóa bằng Public Key, chỉ có máy Client sở hữu Private Key tương ứng mới giải mã và trả lời được. Ưu điểm: Chống brute-force tuyệt đối, không lộ mật khẩu qua mạng.

### **Câu 11: Biện pháp Hardening cho SSH thực tế**
1. **Disable Password Authentication:** Chỉ cho phép đăng nhập bằng cặp khóa (`PasswordAuthentication no`).
2. **Đổi cổng mặc định (Custom Port):** Chuyển từ cổng 22 sang cổng khác để tránh quét tự động từ botnet.
3. **Cấm Root Login trực tiếp:** Bắt buộc đăng nhập tài khoản cá nhân rồi mới dùng `sudo`.

---

## 3. Phần thực hành & Minh chứng hình ảnh
* **Demo tạo cặp khóa SSH (`ssh-keygen`) và xác thực không cần mật khẩu:**

## 4. Tổng kết: Công cụ sử dụng & Khó khăn gặp phải

### **4.1. Các ứng dụng và công cụ đã sử dụng:**
* **Hệ điều hành / Máy ảo:** Ubuntu Linux (chạy trên môi trường ảo hóa VirtualBox).
* **Giao thức quản trị từ xa:** Telnet và SSH (`OpenSSH Server/Client`).
* **Công cụ phân tích mạng:** Wireshark (bắt và phân tích gói tin mạng).
* **Công cụ bảo mật:** `ssh-keygen`, `ssh-copy-id` (cấu hình xác thực Public-Key).
* **Quản lý mã nguồn:** Git & GitHub (lưu trữ và nộp báo cáo bài lab `LAB_AT_BMHTTT`).

### **4.2. Khó khăn gặp phải và hướng giải quyết:**
* **Khó khăn về dịch vụ Telnet:** Quá trình kích hoạt dịch vụ Telnet trên các phiên bản Ubuntu hiện đại gặp xung đột cấu hình siêu dịch vụ `inetd` dẫn đến lỗi từ chối kết nối (`Connection refused`).
  * *Hướng giải quyết:* Tập trung hoàn thiện tối đa và sâu sắc toàn bộ các phần cốt lõi của **SSH** (đã triển khai thành công 100% từ xác thực mật khẩu đến Public-Key và phân tích bảo mật).
* **Khó khăn khi bắt gói tin:** Việc phân tách lưu lượng giữa card mạng ảo loopback và card Wi-Fi thật trên Wireshark đòi hỏi thao tác chính xác.
  * *Hướng giải quyết:* Kết hợp chặt chẽ giữa phân tích lý thuyết chuyên sâu và các câu lệnh thực thi kiểm chứng thực tế trên Terminal.
