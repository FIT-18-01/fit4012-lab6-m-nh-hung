# Threat Model - Lab 6 AES-CBC Socket

## Thông tin nhóm

- Thành viên 1: Bùi Đình Mạnh
- Thành viên 2: 

## Assets

Các tài sản cần bảo vệ trong hệ thống gồm:

- Plaintext trước khi mã hóa.
- AES key dùng cho mã hóa và giải mã.
- IV dùng trong AES-CBC.
- Ciphertext được truyền qua data channel.
- File đầu vào `sample_input.txt`.
- File đầu ra `sample_output.txt`.
- Các file log trong thư mục `logs/`.
- Địa chỉ IP và port sử dụng trong quá trình demo.

## Attacker model

Đối tượng tấn công có thể thực hiện các hành vi sau:

- Nghe lén mạng LAN để bắt gói tin.
- Bắt dữ liệu trên KEY_PORT.
- Bắt dữ liệu trên DATA_PORT.
- Sửa ciphertext trong quá trình truyền.
- Replay lại packet cũ nhiều lần.
- Đọc file log nếu log chứa dữ liệu nhạy cảm.
- Giả mạo Sender để gửi dữ liệu tới Receiver.
- Gửi packet lỗi nhằm làm Receiver bị treo hoặc lỗi xử lý.

## Threats

### 1. Key disclosure

AES key và IV được gửi plaintext qua key channel nên attacker có thể nghe lén và lấy được khóa giải mã.

### 2. Tampering attack

Ciphertext có thể bị sửa đổi trong quá trình truyền qua mạng, dẫn tới lỗi giải mã hoặc dữ liệu sai lệch.

### 3. Replay attack

Attacker có thể gửi lại packet cũ nhiều lần do hệ thống chưa có nonce hoặc timestamp để kiểm tra tính mới của dữ liệu.

### 4. Log leakage

Nếu AES key hoặc IV bị ghi vào log, attacker có thể đọc log và khôi phục dữ liệu đã mã hóa.

### 5. No authentication

Receiver chưa có cơ chế xác thực Sender nên attacker có thể giả mạo Sender và gửi dữ liệu trái phép.

### 6. Denial of Service

Attacker có thể gửi packet sai định dạng hoặc giữ kết nối quá lâu làm Receiver bị treo hoặc chiếm tài nguyên.


## Mitigations

### 1. Secure key exchange

Không gửi AES key plaintext trong hệ thống thực tế. Có thể dùng TLS hoặc cơ chế trao đổi khóa an toàn như Diffie-Hellman.

### 2. Authenticated encryption

Sử dụng AES-GCM thay cho AES-CBC để vừa mã hóa vừa kiểm tra tính toàn vẹn dữ liệu.

### 3. Protect logging system

Không ghi AES key hoặc IV thật vào log trong môi trường production.

### 4. Replay protection

Thêm nonce hoặc timestamp để phát hiện packet cũ bị replay.

### 5. Sender authentication

Bổ sung cơ chế xác thực Sender trước khi Receiver chấp nhận dữ liệu.

### 6. Packet validation

Kiểm tra timeout socket, độ dài header, kích thước ciphertext, AES key và IV để tránh packet lỗi hoặc dữ liệu không hợp lệ.
## Residual risks

Dù đã tách key channel và data channel, hệ thống vẫn chưa an toàn để triển khai thực tế vì key channel chỉ mang tính mô phỏng học tập. Hệ thống chưa có TLS, chưa có xác thực Sender đầy đủ và chưa chống replay attack hoàn chỉnh.
