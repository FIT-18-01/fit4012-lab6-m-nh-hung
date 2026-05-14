# Peer Review Response

## Nội dung đã kiểm tra

Nhóm đã kiểm tra lại toàn bộ luồng hoạt động của hệ thống AES-CBC Socket gồm:

- Sender
- Receiver
- key channel
- data channel
- AES encryption/decryption
- PKCS#7 padding
- packet format
- logging
- pytest

## Điều chỉnh đã thực hiện

- Hoàn thiện README.md
- Hoàn thiện report-1page.md
- Hoàn thiện threat-model-1page.md
- Kiểm tra AES-CBC và packet structure
- Bổ sung log minh chứng
- Kiểm tra negative test cho wrong key và tamper ciphertext

## Kết luận

Nhóm đã hoàn thiện các yêu cầu chính của Lab 6 và kiểm tra lại hệ thống bằng pytest cũng như demo thực tế giữa Sender và Receiver.