# Báo cáo Bài Lab An Toàn Hệ Thống Thông Tin - LAB 4

- **Họ và tên**: Trần Thanh Phương
- **Mã số sinh viên (MSSV)**: 1150080165
- **Tên lab**: Thực hành quét mạng, đánh giá lỗ hổng và Hardening với Nmap / Metasploitable 2
- **Phiên bản môi trường**: Windows 11 (Host / Zenmap 7.991) & VMware Workstation (Metasploitable 2 VM)

## Cách dựng môi trường
1. Khởi động phần mềm ảo hóa VMware Workstation và chạy máy ảo mục tiêu Metasploitable 2.
2. Cấu hình card mạng máy ảo ở chế độ Host-Only (VMnet8) để thiết lập kết nối mạng nội bộ thông suốt giữa máy thật (Host) và máy ảo (Target).
3. Sử dụng công cụ Zenmap / Nmap trên hệ điều hành Windows để thực hiện các kịch bản kiểm tra an toàn và quét mạng.

## Các tình huống đã thực hiện
- Thực hiện Host Discovery để xác định trạng thái máy chủ trong dải mạng.
- Khảo sát các cổng TCP thông qua các kỹ thuật quét: TCP Connect (`-sT`), SYN (`-sS`), FIN, Xmas, NULL và ACK scan.
- Tiến hành quét UDP có kiểm soát (`-sU`) trên top các cổng phổ biến.
- Nhận diện phiên bản dịch vụ (`-sV`), hệ điều hành (`-O`) và chạy Aggressive scan (`-A`).
- Kiểm tra thông tin SMB và dò quét lỗ hổng MS17-010 bằng Nmap Scripting Engine (NSE).
- Thực hiện kịch bản đánh giá tình huống trước và sau khi Hardening (Before/After).

## Kết quả thực hiện
- **Trạng thái**: PASS
- Toàn bộ kết quả chi tiết, hình ảnh minh chứng thực tế từ máy cá nhân và bảng số liệu đã được tổng hợp đầy đủ trong file báo cáo Word (`.docx`) kèm theo.

## Lỗi gặp phải và cách khắc phục
- **Lỗi**: Máy chủ Metasploitable 2 chặn gói tin ICMP Ping mặc định khiến Nmap nhận diện nhầm host ở trạng thái "down" và dừng quét.
- **Cách khắc phục**: Thêm tham số `-Pn` vào tất cả các câu lệnh Nmap để ép công cụ bỏ qua bước kiểm tra ping và tiến hành quét trực tiếp thành công.
