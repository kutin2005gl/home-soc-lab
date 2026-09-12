# Buổi 01 — Mạng máy tính cơ bản

**Ngày:** 12/09/2026
**Thời lượng:** ___ giờ
**Phase / mục:** Phase 0 → 0.1 Mạng máy tính

---

## Đã học được gì

### Mô hình đóng gói 4 tầng

Dữ liệu không gửi thẳng đi mà được bọc nhiều lớp, mỗi lớp thêm một loại địa chỉ:

| Tầng | Trả lời câu hỏi | Thông tin thêm vào |
|---|---|---|
| 7 — Ứng dụng | Nội dung là gì? | `GET / HTTP/1.1` |
| 4 — TCP | Đến ứng dụng nào trên máy đó? | Port nguồn, port đích |
| 3 — IP | Đến máy nào trên Internet? | IP nguồn, IP đích |
| 2 — Ethernet | Chuyển tới thiết bị kế bên nào? | Địa chỉ MAC |

Ý quan trọng nhất: **MAC thay đổi qua mỗi chặng router, IP giữ nguyên suốt hành
trình.** Đó là lý do log an ninh ghi IP chứ hiếm khi ghi MAC.

### Dải IP riêng (private)

- `10.0.0.0` – `10.255.255.255`
- `172.16.0.0` – `172.31.255.255`
- `192.168.0.0` – `192.168.255.255`

Ứng dụng vào SOC: nhìn hướng kết nối để biết chuyện gì đang xảy ra.
Nội bộ → công cộng là máy trong nhà gọi ra ngoài.
Công cộng → nội bộ là từ ngoài gọi vào.

### CIDR

`192.168.1.15/24` — 24 bit đầu (`192.168.1`) là phần mạng, số cuối là phần máy.
Mọi máy `192.168.1.1` đến `192.168.1.254` cùng mạng, nói chuyện trực tiếp được.
Ra khỏi mạng phải đi qua gateway.

### Port cần thuộc

| Port | Dịch vụ | Ý nghĩa với SOC |
|---|---|---|
| 22 | SSH | Quản trị Linux từ xa, hay bị brute force |
| 53 | DNS | Malware giấu dữ liệu trong truy vấn DNS |
| 80 | HTTP | Không mã hoá, đọc được nội dung |
| 443 | HTTPS | Mã hoá, C2 hay giả dạng traffic này |
| 445 | SMB | Chia sẻ file Windows, lateral movement, ransomware |
| 3389 | RDP | Remote Desktop, mục tiêu tấn công hàng đầu |

### TCP three-way handshake

`SYN` → `SYN-ACK` → `ACK`

Ứng dụng: nhiều `SYN` đến nhiều port khác nhau mà không có `ACK` theo sau chính
là **port scan**. Đây sẽ là một trong những rule đầu tiên tự viết ở Phase 5.

### Hành trình một request đầy đủ

1. Hỏi DNS tên miền → nhận IP
2. So sánh subnet, xác định gửi thẳng hay qua gateway
3. Bắt tay TCP với port 443
4. Bắt tay TLS, trao đổi khoá
5. Gửi `GET / HTTP/1.1`
6. Nhận `200 OK` kèm nội dung
7. Đóng kết nối bằng `FIN` / `ACK`

---

## Kết quả thực hành

### Bài 1 — IP và gateway của máy mình
```
(dán output ip a / ipconfig, che phần nhạy cảm)
```
- IP: 
- Subnet mask: 
- Gateway: 

### Bài 2 — Phân giải DNS
```
(dán output nslookup example.com)
```

### Bài 3 — Traceroute
```
(dán output)
```
- Số chặng đi qua: 

### Bài 4 — Kết nối đang mở
```
(dán output ss -tunap / netstat -ano)
```
- Tìm được dòng nào dùng port 443? 
- Tiến trình nào đang giữ kết nối đó? 

### Bài 5 — HTTP request thủ công
```
(dán output curl -v http://example.com)
```

---

## Bị vướng ở đâu

- 

## Cách đã giải quyết

- 

## Câu hỏi còn bỏ ngỏ

- 

## Buổi sau làm gì

- Học DNS chi tiết hơn
- Phân biệt TCP và UDP
- Tìm hiểu NAT
- Cài Wireshark, bắt gói tin thật, nhìn tận mắt SYN / SYN-ACK / ACK
