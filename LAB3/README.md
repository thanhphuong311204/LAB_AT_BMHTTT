# BÁO CÁO THỰC HÀNH LAB 3: GIÁM SÁT HỆ THỐNG VÀ MẠNG

- **Họ và tên:** Trần Thanh Phương
- **MSSV:** 1150080165
- **Mã lớp:** 11CNPM1 
- **Môi trường thực hành:** Windows 11 x64, Sysmon v15.22, Sysinternals Suite, Wireshark v4.6.8 / Windows Pktmon.

## 1. Cách dựng môi trường
- Thiết lập thư mục làm việc tại `C:\LAB3`.
- Cấu hình và kích hoạt dịch vụ Sysmon theo dõi Process Create, Network Connection.
- Cấu hình HTTP Server cục bộ bằng Python lắng nghe tại `127.0.0.1:8080`.

## 2. Kết quả thực hiện các tình huống
- **Tình huống 1 - 3 (Cấu hình & Event Log):** PASS
- **Tình huống 4 (Sysinternals - Autoruns, Process Explorer):** PASS
- **Tình huống 5 (Giám sát mạng & HTTP Traffic):** PASS

## 3. Lỗi gặp phải và cách khắc phục
- **Lỗi 1 (Sysmon access denied):** Do phiên làm việc thiếu quyền Elevated Admin. Khắc phục bằng cách mở PowerShell qua *Run as Administrator*.
- **Lỗi 2 (Wireshark thiếu Npcap driver trên VM cô lập):** Do máy ảo không có kết nối Internet để tải Npcap. Khắc phục bằng cách sử dụng công cụ nội tại `pktmon` (Packet Monitor) của Windows để capture gói tin và chuyển đổi sang `.pcapng` mở trên Wireshark.
