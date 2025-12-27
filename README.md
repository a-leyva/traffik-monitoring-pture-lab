# 🧩 Network Traffic Analysis with Wireshark

### **Objective**
Capture and analyze web application traffic on a local Juice Shop instance to identify sensitive information transmitted in plaintext and demonstrate packet-level visibility using Wireshark.

---

### **Environment**
- **System:** Kali Linux (host)
- **Target:** OWASP Juice Shop (localhost:3000)
- **Tool:** Wireshark + tcpdump
- **Date:** 2025-11-11

---

### **Procedure**
1. Started Juice Shop on localhost:3000.
2. Captured loopback traffic using:
   ```bash
   sudo tcpdump -i lo port 3000 -w ~/20251110_wireshark_capture.pcap
   ```
3. Opened capture in Wireshark and applied filters:
   ```
   http
   http.request.method == "POST"
   tcp.port == 3000
   tcp.len > 0
   ```
4. Inspected packet statistics via “Statistics → Conversations”.
5. Followed an HTTP Stream for a POST login request to view transmitted credentials.
6. Documented findings and mitigation steps.

---

### **Findings**
- Captured 961 packets, filtered down to 127 with HTTP data.
- Observed HTTP POST requests to `/rest/user/login`.
- Plaintext email and password visible in packet payload:
  ```
  "email":"test1234","password":"test1234"
  ```
- Server responded with 401 Unauthorized (invalid credentials).
- Network analysis confirmed lack of encryption (no TLS handshake packets).

📸 **Evidence:**
- Screenshot_2025-11-11_00-31-05_tcpstream.png (Follow HTTP Stream)
- Screenshot_2025-11-11_00-29-54_conversations.png (Conversation Stats)

---

### **Impact**
- Sensitive credentials transmitted in plaintext.
- Susceptible to interception or sniffing.
- Demonstrates risk of using HTTP instead of HTTPS.

---

### **Mitigation**
- Implement HTTPS (TLS 1.2+) on all web endpoints.
- Use secure cookies with HttpOnly and Secure flags.
- Avoid transmitting credentials directly in body without encryption.
- Enable Content Security Policy (CSP) and X-Frame-Options.

---

### **Lessons Learned**
- HTTP traffic reveals valuable data during network monitoring.
- Wireshark provides deep insight into insecure implementations.
- Encryption (TLS) is essential for confidentiality and integrity.

---
