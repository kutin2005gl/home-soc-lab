# home-soc-lab

Xây dựng một Security Operations Center thu nhỏ tại nhà để học **SIEM, phân tích log
và viết detection rule** bằng cách tự tấn công rồi tự phát hiện.

**Trạng thái:** 🟡 Đang thực hiện — Phase 0 (Nền tảng)
**Bắt đầu:** 12/09/2026
**Thời lượng dự kiến:** 6 tháng

---

## Mục tiêu

1. Dựng được một lab SOC hoạt động thật với SIEM tập trung
2. Hiểu được đường đi của một dòng log: từ endpoint đến alert
3. Tự viết và tinh chỉnh được detection rule, map về MITRE ATT&CK
4. Tự mô phỏng tấn công trong lab cách ly để kiểm chứng độ phủ detection
5. Viết được báo cáo điều tra sự cố theo chuẩn nghề

---

## Kiến trúc lab

Toàn bộ chạy trên VirtualBox, mạng Host-only cách ly hoàn toàn với Internet.

```
                    ┌─────────────────────────┐
                    │   Wazuh Server (SIEM)   │
                    │   Ubuntu 22.04 · .10    │
                    │   Dashboard, rule, alert│
                    └────────────┬────────────┘
                                 │  log
              ┌──────────────────┼──────────────────┐
              │                  │                  │
     ┌────────┴───────┐ ┌────────┴───────┐ ┌────────┴───────┐
     │  Windows 10    │ │ Ubuntu Server  │ │  Kali Linux    │
     │      .20       │ │      .30       │ │      .40       │
     │ Agent + Sysmon │ │ Agent + auditd │ │   Attacker     │
     └────────────────┘ └────────────────┘ └────────────────┘

              Host-only network · 192.168.56.0/24
              Không NAT · Snapshot trước mỗi thử nghiệm
```

| Máy | IP | RAM | Vai trò |
|---|---|---|---|
| Wazuh Server | 192.168.56.10 | 4 GB | SIEM, nhận và phân tích log |
| Windows 10 | 192.168.56.20 | 4 GB | Endpoint nạn nhân, Sysmon |
| Ubuntu Server | 192.168.56.30 | 1 GB | Endpoint nạn nhân Linux |
| Kali Linux | 192.168.56.40 | 2 GB | Máy tấn công (purple team) |

---

## Công nghệ sử dụng

| Thành phần | Công cụ |
|---|---|
| SIEM | Wazuh |
| Endpoint telemetry | Sysmon, Windows Event Log, auditd |
| Network IDS | Suricata |
| Network metadata | Zeek |
| Detection format | Sigma |
| Threat framework | MITRE ATT&CK |
| Attack simulation | Atomic Red Team |

---

## Cấu trúc repo

```
├── roadmap.md              Lộ trình học 24 tuần, có checkbox
├── journal/                Nhật ký từng buổi học
├── lab/
│   ├── network-design.md   Sơ đồ mạng, bảng IP
│   └── setup/              Ghi chú cài đặt từng thành phần
├── detections/
│   ├── sigma/              Rule Sigma tự viết
│   ├── wazuh/              local_rules.xml
│   └── README.md           Bảng map rule ↔ MITRE technique
├── investigations/         Báo cáo điều tra từng case
└── notes/                  Cheatsheet tự tổng hợp
```

---

## Tiến độ

| Phase | Nội dung | Thời lượng | Trạng thái |
|---|---|---|---|
| 0 | Nền tảng: mạng, Linux, Windows, ảo hoá | 6 tuần | 🟡 Đang làm |
| 1 | Nền tảng an toàn thông tin, MITRE ATT&CK | 2 tuần | ⬜ Chưa |
| 2 | Dựng lab, cài Wazuh + agent + Sysmon | 2 tuần | ⬜ Chưa |
| 3 | Đọc hiểu log Windows, Sysmon, Linux | 3 tuần | ⬜ Chưa |
| 4 | Truy vấn và điều tra trên SIEM | 2 tuần | ⬜ Chưa |
| 5 | Viết và tinh chỉnh detection rule | 4 tuần | ⬜ Chưa |
| 6 | Giám sát mạng: Suricata, Zeek | 2 tuần | ⬜ Chưa |
| 7 | Purple team, báo cáo, tổng kết | 3 tuần | ⬜ Chưa |

### Chi tiết Phase 0

- [x] Mô hình TCP/IP 4 tầng
- [x] Địa chỉ IP, subnet mask, CIDR
- [x] Các port quan trọng: 22, 53, 80, 443, 445, 3389
- [x] TCP three-way handshake
- [ ] Private IP vs Public IP, NAT
- [ ] Phân biệt TCP và UDP
- [ ] DNS hoạt động chi tiết
- [ ] HTTP request/response, status code
- [ ] Wireshark: bắt và đọc gói tin
- [ ] Linux command line và log
- [ ] Windows internals và Event Viewer
- [ ] Ảo hoá và thiết kế mạng lab

---

## Detection coverage

Bảng này sẽ được điền dần từ Phase 5.

| Rule | MITRE Technique | Nguồn log | Trạng thái |
|---|---|---|---|
| _(chưa có)_ | | | |

---

## Nguyên tắc an toàn

1. Máy nạn nhân luôn ở mạng cách ly, không nối Internet khi thử nghiệm
2. Snapshot trước mỗi lần thử, khôi phục sau khi xong
3. Không chạy mẫu độc hại thật trên máy host
4. Chỉ tấn công hệ thống trong lab của mình — tấn công hệ thống khác là phạm pháp
5. Không commit IP công cộng thật, hostname thật hay thông tin mạng cá nhân.
   Trong tài liệu dùng dải ví dụ `203.0.113.0/24`

---

## Nhật ký gần nhất

| Buổi | Ngày | Nội dung |
|---|---|---|
| 01 | 12/09/2026 | Mạng cơ bản: mô hình 4 tầng, IP, subnet, port, TCP handshake |
