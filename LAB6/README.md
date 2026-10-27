
*Trần Thanh Phương -1150080165 *
# LAB 6: pfSense Advanced Firewall, Captive Portal & LDAP Authentication

Kho lưu trữ này chứa toàn bộ tài liệu, cấu hình hệ thống và hình ảnh minh chứng kết quả thực hiện **Lab 6** môn Quản trị mạng / An toàn hệ thống thông tin.

##  Môi trường thực thi & Topology
* **Hệ thống ảo hóa**: Oracle VM VirtualBox.
* **Tường lửa**: pfSense Community Edition (`pfSense-LAB6`) với 4 giao diện mạng (WAN, MGMT, LAN, DMZ).
* **Môi trường tích hợp**: Windows Server (Active Directory - DC01 tại `10.6.0.10`) và các máy trạm kiểm thử (`CLIENT_A`, `CLIENT_B`).

---

## Các hạng mục cấu hình chính trong Lab

1. **Xác thực người dùng (LDAP Authentication)**:
   * Tích hợp pfSense với Active Directory thông qua giao thức LDAP (`AD-LDAP`).
   * Kiểm thử xác thực thành công cho các tài khoản người dùng miền (`sv_a`, `sv_b`).

2. **Captive Portal**:
   * Kích hoạt và cấu hình Captive Portal trên giao diện mạng **LAN** kết hợp với backend xác thực LDAP.

3. **Tạo Aliases**:
   * **IP Aliases (7 đối tượng)**: `CLIENT_A`, `CLIENT_B`, `POLICY_CLIENTS`, `GENERAL_SITE`, `JOB_SITE`, `SOCIAL_SITE`, `RESTRICTED_SITE`.
   * **Port Aliases (1 đối tượng)**: `WEB_PORTS` (bao gồm cổng 80 và 443).

4. **Lịch trình hoạt động (Schedules)**:
   * `LUNCH_WINDOW`: Khung giờ nghỉ trưa (Thứ Hai đến Thứ Sáu, từ 11:30 đến 13:00).
   * `WORK_HOURS`: Khung giờ hành chính (Thứ Hai đến Thứ Sáu, từ 08:00 đến 17:00).

5. **Chính sách Tường lửa (Firewall Rules)**:
   * **DMZ Tab**: Thiết lập quy tắc cô lập hoàn toàn lưu lượng từ phân đoạn DMZ sang mạng LAN.
   * **LAN Tab**: Xây dựng hệ thống 7 quy tắc chi tiết theo nguyên tắc ưu tiên từ trên xuống (First-match), quản lý quyền truy cập web, chặn các trang mạng xã hội/việc làm ngoài giờ và áp dụng lịch trình linh hoạt.
