# Lộ trình học Home SOC Lab — từ con số 0

> Tài liệu cá nhân. Mỗi buổi học mở file này ra, tick checkbox đã làm xong,
> và ghi vào phần Nhật ký ở cuối.

**Tổng thời lượng dự kiến:** 6 tháng, học 5–7 giờ/tuần
(nếu học 15 giờ/tuần thì rút xuống khoảng 3 tháng)

**Mục tiêu cuối cùng:** có một lab SOC tại nhà chạy được, tự phát hiện được
tấn công mình tạo ra, và có một bộ hồ sơ mang đi phỏng vấn vị trí SOC Analyst L1.

---

## Cách dùng tài liệu này

1. **Không nhảy cóc.** Mỗi giai đoạn là nền của giai đoạn sau. Bỏ Phase 0 thì
   đến Phase 3 sẽ không hiểu tại sao log lại quan trọng.
2. **Mỗi mục có một "bài kiểm tra thật".** Không tick checkbox cho đến khi làm
   được bài đó mà không nhìn hướng dẫn.
3. **Quy tắc 70/30.** 70% thời gian gõ tay trên máy, 30% đọc/xem video. Học
   security bằng cách đọc là học giả.
4. **Ghi lại mọi thứ.** Phần Nhật ký cuối file chính là portfolio của bạn.

---

## PHASE 0 — Nền tảng bắt buộc (6 tuần)

Đây là phần chán nhất nhưng quyết định bạn có đi được xa hay không.

### 0.1 — Mạng máy tính (2 tuần)

Bạn không cần học để thi chứng chỉ, chỉ cần hiểu đủ để đọc được một dòng log mạng.

- [ ] Mô hình TCP/IP 4 tầng — biết mỗi tầng làm gì
- [ ] Địa chỉ IP, subnet mask, CIDR (`192.168.1.0/24` nghĩa là gì)
- [ ] Private IP vs Public IP, NAT
- [ ] Port là gì, phân biệt TCP và UDP
- [ ] Thuộc lòng các port: 22 SSH, 80 HTTP, 443 HTTPS, 445 SMB, 3389 RDP, 53 DNS
- [ ] DNS hoạt động thế nào (từ lúc gõ tên miền đến lúc có IP)
- [ ] TCP three-way handshake (SYN, SYN-ACK, ACK)
- [ ] HTTP request/response, status code, header
- [ ] Cài Wireshark, bắt gói tin khi truy cập một website, tìm ra gói DNS query

**Bài kiểm tra thật:** Mở Wireshark, truy cập một trang HTTP bất kỳ, rồi chỉ ra
được: gói DNS query, 3 gói bắt tay TCP, gói HTTP GET đầu tiên.

**Tài nguyên:**
- Professor Messer — Network+ (YouTube, miễn phí, đủ dùng)
- Sách: "Practical Packet Analysis" — Chris Sanders

---

### 0.2 — Linux (2 tuần)

- [ ] Cài Ubuntu Server trong VirtualBox, không dùng giao diện đồ hoạ
- [ ] Di chuyển thư mục: `cd`, `ls -la`, `pwd`, `find`
- [ ] Đọc file: `cat`, `less`, `head`, `tail -f`
- [ ] Lọc dữ liệu: `grep`, `awk`, `cut`, `sort`, `uniq -c`, pipe `|`
- [ ] Quyền: `chmod`, `chown`, hiểu `rwx` và `sudo`
- [ ] Tiến trình: `ps aux`, `top`, `kill`, `systemctl status`
- [ ] Mạng: `ip a`, `ss -tulpn`, `ping`, `curl`
- [ ] Cấu trúc thư mục: `/etc`, `/var/log`, `/home`, `/tmp`
- [ ] Sửa file bằng `nano` hoặc `vim`
- [ ] Hiểu `/var/log/auth.log` chứa gì

**Bài kiểm tra thật:** Từ file `/var/log/auth.log`, dùng một dòng lệnh duy nhất
để đếm số lần đăng nhập thất bại theo từng IP, sắp xếp giảm dần.

```bash
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -rn
```

Đừng chép dòng trên. Tự viết ra trước, rồi so sánh.

**Tài nguyên:** OverTheWire Bandit (game, level 0→20), Linux Journey

---

### 0.3 — Windows nội bộ (1 tuần)

SOC chủ yếu xử lý log Windows, nên phần này không được bỏ.

- [ ] Mở Event Viewer, xem qua Security / System / Application log
- [ ] Hiểu Process, Parent Process, Service, Scheduled Task là gì
- [ ] Registry là gì, mở `regedit`, xem `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`
- [ ] Tài khoản local vs domain, nhóm Administrators
- [ ] PowerShell cơ bản: `Get-Process`, `Get-Service`, `Get-WinEvent`
- [ ] Cài Sysinternals Suite, dùng Process Explorer và Autoruns

**Bài kiểm tra thật:** Tạo một user mới trên Windows, sau đó tìm được chính xác
sự kiện ghi lại việc đó trong Event Viewer (gợi ý: Event ID 4720).

---

### 0.4 — Ảo hoá (3 ngày)

- [ ] Cài VirtualBox
- [ ] Tạo VM Ubuntu, VM Windows 10 (dùng bản đánh giá 90 ngày của Microsoft)
- [ ] Hiểu các chế độ mạng: NAT, Bridged, Host-only, Internal Network
- [ ] Biết dùng Snapshot — chụp trước, khôi phục sau khi thử nghiệm

**Bài kiểm tra thật:** Hai VM ping được nhau qua Host-only network, nhưng không
ra được Internet.

---

## PHASE 1 — Nền tảng an toàn thông tin (2 tuần)

- [ ] Bộ ba CIA: Confidentiality, Integrity, Availability
- [ ] Phân biệt: vulnerability, threat, exploit, risk
- [ ] Malware các loại: virus, worm, trojan, ransomware, RAT
- [ ] Các kiểu tấn công phổ biến: phishing, brute force, privilege escalation,
      lateral movement, data exfiltration
- [ ] Đọc hết MITRE ATT&CK Enterprise Matrix một lượt (chỉ cần biết có những
      tactic nào: Initial Access → Execution → Persistence → ... → Impact)
- [ ] Cyber Kill Chain
- [ ] SOC là gì, phân biệt vai trò L1 / L2 / L3
- [ ] Hiểu quy trình xử lý sự cố: Detect → Triage → Investigate → Contain → Recover

**Bài kiểm tra thật:** Chọn một nhóm tấn công bất kỳ trên MITRE ATT&CK
(ví dụ APT29), đọc hồ sơ của họ và viết ra 5 technique họ hay dùng bằng lời của
mình.

**Tài nguyên:** TryHackMe — lộ trình "Pre Security" và "SOC Level 1" (rất phù hợp
cho người mới, có phần miễn phí)

---

## PHASE 2 — Dựng lab (2 tuần)

### 2.1 Thiết kế mạng lab

Nguyên tắc an toàn: **máy victim không bao giờ được nối trực tiếp Internet khi
đang chạy thử nghiệm độc hại.** Luôn snapshot trước.

Sơ đồ mạng đề xuất — dải `192.168.56.0/24` (Host-only):

| VM | IP | RAM | Vai trò |
|---|---|---|---|
| Wazuh Server (Ubuntu 22.04) | .10 | 4GB | SIEM |
| Windows 10 | .20 | 4GB | Victim, có agent + Sysmon |
| Ubuntu Server | .30 | 1GB | Victim Linux |
| Kali Linux | .40 | 2GB | Máy tấn công |

Nếu chỉ có 8GB RAM: bật Wazuh + Windows trước, Kali bật sau khi cần.

- [ ] Tạo Host-only network trong VirtualBox
- [ ] Đặt IP tĩnh cho từng VM
- [ ] Kiểm tra tất cả ping được nhau
- [ ] Snapshot tất cả VM ở trạng thái sạch, đặt tên `clean-baseline`

### 2.2 Cài SIEM

- [ ] Cài Wazuh all-in-one trên VM Ubuntu:
      `curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh && sudo bash ./wazuh-install.sh -a`
- [ ] Lưu lại mật khẩu admin mà script in ra
- [ ] Truy cập dashboard tại `https://192.168.56.10`
- [ ] Cài Wazuh agent lên Windows, trỏ manager về `192.168.56.10`
- [ ] Cài Wazuh agent lên Ubuntu victim
- [ ] Xác nhận cả 2 agent hiện trạng thái Active trên dashboard

### 2.3 Bật log chi tiết

- [ ] Tải Sysmon (Sysinternals) + config của SwiftOnSecurity
- [ ] Cài: `sysmon64.exe -accepteula -i sysmonconfig-export.xml`
- [ ] Sửa `C:\Program Files (x86)\ossec-agent\ossec.conf`, thêm channel
      `Microsoft-Windows-Sysmon/Operational`
- [ ] Bật Command Line Auditing qua `gpedit.msc` để Event ID 4688 có command line
- [ ] Bật audit Process Creation trong Advanced Audit Policy
- [ ] Restart agent, xác nhận log Sysmon đã về dashboard

**Bài kiểm tra thật:** Mở `cmd.exe` trên máy Windows, trong vòng 1 phút tìm được
sự kiện Sysmon Event ID 1 tương ứng trên dashboard Wazuh, kèm command line đầy đủ.

---

## PHASE 3 — Đọc hiểu log (3 tuần)

Đây là kỹ năng lõi. Học chậm, học kỹ.

### 3.1 Windows Security Log

Mỗi ngày học 3 Event ID: đọc nó ghi gì, rồi **tự tạo ra sự kiện đó** và tìm lại log.

- [ ] `4624` Đăng nhập thành công — học kỹ field Logon Type
      (2=local, 3=network, 10=RDP)
- [ ] `4625` Đăng nhập thất bại
- [ ] `4634` / `4647` Đăng xuất
- [ ] `4672` Cấp quyền đặc biệt (dấu hiệu admin logon)
- [ ] `4688` Tạo tiến trình mới
- [ ] `4720` Tạo user mới
- [ ] `4726` Xoá user
- [ ] `4732` Thêm user vào nhóm có quyền
- [ ] `4768` / `4769` Kerberos ticket (quan trọng với AD)
- [ ] `5140` Truy cập network share
- [ ] `7045` Cài service mới — kỹ thuật persistence rất phổ biến
- [ ] `1102` Xoá Security log — gần như luôn là dấu hiệu xấu

### 3.2 Sysmon

- [ ] ID `1` Process creation (có hash, parent, command line)
- [ ] ID `3` Network connection kèm tên process
- [ ] ID `7` Load DLL
- [ ] ID `8` CreateRemoteThread — process injection
- [ ] ID `10` Process access — dấu hiệu dump LSASS
- [ ] ID `11` Tạo file
- [ ] ID `12`/`13`/`14` Thao tác registry
- [ ] ID `22` DNS query

### 3.3 Linux

- [ ] Đọc `/var/log/auth.log` — SSH thành công, thất bại, `sudo`
- [ ] Cài và cấu hình `auditd`
- [ ] Viết audit rule theo dõi thay đổi `/etc/passwd`
- [ ] Đọc log `auditd` bằng `ausearch`

### 3.4 Khái niệm chuẩn hoá

- [ ] Hiểu vì sao cần normalization (Windows gọi user là `TargetUserName`,
      Linux gọi là `acct`, Zeek gọi IP nguồn là `id.orig_h`)
- [ ] Tìm hiểu ECS (Elastic Common Schema)
- [ ] Xem decoder của Wazuh trong `/var/ossec/ruleset/decoders/`

**Bài kiểm tra thật:** Cho một dòng log Windows 4624 bất kỳ, giải thích được:
ai đăng nhập, từ máy nào, kiểu đăng nhập gì, có phải tài khoản đặc quyền không.

---

## PHASE 4 — Truy vấn và điều tra (2 tuần)

- [ ] Học cú pháp tìm kiếm trên Wazuh Dashboard (Lucene / KQL)
- [ ] Lọc theo field: `data.win.eventdata.image: "*\\powershell.exe"`
- [ ] Kết hợp điều kiện: `AND`, `OR`, `NOT`
- [ ] Lọc theo khoảng thời gian
- [ ] Dùng wildcard và so khớp một phần
- [ ] Tạo visualization: top 10 process được chạy nhiều nhất
- [ ] Tạo dashboard riêng cho máy Windows
- [ ] Học kỹ thuật pivot: từ một alert, tìm ra tất cả hoạt động liên quan của
      cùng user / cùng máy / cùng khung giờ

**Bài kiểm tra thật:** Từ một alert đăng nhập thất bại, tìm ra toàn bộ hoạt động
của IP đó trong 1 giờ trước và sau thời điểm đó.

---

## PHASE 5 — Viết và tinh chỉnh detection rule (4 tuần)

### 5.1 Rule cơ bản

- [ ] Hiểu cấu trúc rule Wazuh trong `/var/ossec/etc/rules/local_rules.xml`
- [ ] Hiểu rule level (0–15) và ý nghĩa từng mức
- [ ] Viết rule đầu tiên: cảnh báo khi có user mới được tạo
- [ ] Viết rule dựa trên ngưỡng: 5 lần `4625` trong 60 giây từ cùng IP
- [ ] Viết rule phát hiện `whoami.exe` có parent là `WINWORD.EXE`
- [ ] Viết rule phát hiện tạo scheduled task
- [ ] Viết rule phát hiện xoá Security log (`1102`), level 15

### 5.2 Tinh chỉnh

Đây là phần phân biệt người biết dùng SIEM và người không.

- [ ] Hiểu True Positive / False Positive / False Negative
- [ ] Cố tình tạo ra một rule kêu quá nhiều, rồi thêm điều kiện loại trừ
- [ ] Viết danh sách allowlist cho hoạt động quản trị hợp lệ
- [ ] Hiểu tại sao alert fatigue là nguyên nhân số 1 khiến SOC bỏ sót tấn công

### 5.3 Sigma và ATT&CK

- [ ] Hiểu cấu trúc một rule Sigma (logsource, detection, condition, level)
- [ ] Cài `sigma-cli`, convert một rule Sigma sang định dạng Wazuh
- [ ] Duyệt kho rule của SigmaHQ trên GitHub, chọn 5 rule và hiểu chúng
- [ ] Map mọi rule mình viết về một MITRE technique ID
- [ ] Dùng ATT&CK Navigator để tô màu những technique mình đã phát hiện được

**Ví dụ rule Sigma mẫu:**

```yaml
title: Whoami executed by Office application
status: experimental
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    ParentImage|endswith:
      - '\WINWORD.EXE'
      - '\EXCEL.EXE'
    Image|endswith: '\whoami.exe'
  condition: selection
falsepositives:
  - Rất hiếm, gần như luôn đáng nghi
level: high
tags:
  - attack.discovery
  - attack.t1033
```

**Bài kiểm tra thật:** Viết 10 rule của riêng mình, mỗi rule map về một technique
ATT&CK khác nhau, và chứng minh từng cái hoạt động bằng cách tự kích hoạt nó.

---

## PHASE 6 — Giám sát mạng (2 tuần)

- [ ] Cài Suricata trên một VM, đặt ở chế độ IDS
- [ ] Nạp bộ rule Emerging Threats (miễn phí)
- [ ] Đẩy log Suricata vào Wazuh
- [ ] Cài Zeek, tìm hiểu `conn.log`, `dns.log`, `http.log`, `ssl.log`
- [ ] Phân tích một file PCAP có sẵn bằng Zeek
- [ ] Hiểu khái niệm beaconing (C2 gọi về theo chu kỳ đều đặn)
- [ ] Nhận diện DNS tunneling qua đặc điểm truy vấn bất thường

**Bài kiểm tra thật:** Từ Kali, quét port máy Windows bằng `nmap`. Tìm được alert
tương ứng trong Suricata và bản ghi kết nối trong `conn.log` của Zeek.

---

## PHASE 7 — Purple team và tổng kết (3 tuần)

### 7.1 Mô phỏng tấn công để kiểm chứng detection

Chỉ chạy trong lab đã cách ly, đã snapshot.

- [ ] Cài Atomic Red Team trên máy Windows victim
- [ ] Chạy test cho T1059 (Command and Scripting Interpreter)
- [ ] Chạy test cho T1053 (Scheduled Task)
- [ ] Chạy test cho T1136 (Create Account)
- [ ] Chạy test cho T1070.001 (Clear Windows Event Logs)
- [ ] Với mỗi test: ghi lại **có bắt được không**, nếu không thì tại sao
- [ ] Viết rule bổ sung cho những cái bị lọt

### 7.2 Kịch bản hoàn chỉnh

- [ ] Dựng một chuỗi tấn công đầy đủ: initial access → execution →
      persistence → discovery → lateral movement
- [ ] Điều tra lại toàn bộ chuỗi đó chỉ bằng SIEM, không nhìn note
- [ ] Dựng timeline sự cố theo thứ tự thời gian

### 7.3 Sản phẩm đầu ra

- [ ] Viết báo cáo sự cố mẫu (executive summary, timeline, IOC, khuyến nghị)
- [ ] Đưa toàn bộ rule của mình lên GitHub
- [ ] Viết README mô tả kiến trúc lab kèm sơ đồ
- [ ] Chụp ảnh ATT&CK Navigator thể hiện độ phủ detection
- [ ] Viết 3 bài blog về 3 detection tâm đắc nhất

---

## Sau khi xong — hướng đi tiếp

- Thêm Active Directory vào lab (Domain Controller + 2 máy join domain)
- Học Splunk (kỹ năng có giá trị cao khi đi làm), luyện SPL
- Threat hunting chủ động thay vì chờ alert
- Chứng chỉ tham khảo: CompTIA Security+ → BTL1 (Blue Team Level 1) → CySA+
- Luyện điều tra thực tế trên CyberDefenders và LetsDefend

---

## Nhật ký học tập

Chép mẫu dưới đây xuống mỗi buổi học. Đây chính là bằng chứng năng lực của bạn.

```
### Buổi số: ___    Ngày: ___/___/______    Thời lượng: ___ giờ

Phase / mục đang học:

Đã làm được gì:
-

Lệnh / cấu hình quan trọng đã dùng:
```
(dán lệnh vào đây)
```

Bị vướng ở đâu:
-

Cách đã giải quyết:
-

Câu hỏi còn bỏ ngỏ, buổi sau phải trả lời:
-

Buổi sau làm gì:
-
```

---

## Bảng theo dõi tiến độ tổng

| Phase | Nội dung | Thời lượng | Ngày bắt đầu | Ngày xong |
|---|---|---|---|---|
| 0 | Nền tảng (mạng, Linux, Windows, VM) | 6 tuần | | |
| 1 | Nền tảng an toàn thông tin | 2 tuần | | |
| 2 | Dựng lab + cài SIEM | 2 tuần | | |
| 3 | Đọc hiểu log | 3 tuần | | |
| 4 | Truy vấn và điều tra | 2 tuần | | |
| 5 | Viết và tinh chỉnh rule | 4 tuần | | |
| 6 | Giám sát mạng | 2 tuần | | |
| 7 | Purple team và tổng kết | 3 tuần | | |

---

## Nguyên tắc an toàn khi làm lab

1. Máy victim luôn ở mạng cách ly (Host-only hoặc Internal Network)
2. Snapshot trước mỗi lần thử nghiệm, khôi phục sau khi xong
3. Không bao giờ chạy mẫu độc hại thật trên máy host
4. Không tấn công bất kỳ hệ thống nào không thuộc lab của mình — việc đó là phạm
   pháp, kể cả khi bạn "chỉ thử"
5. Mọi tài khoản trong lab dùng mật khẩu riêng, không trùng tài khoản cá nhân
