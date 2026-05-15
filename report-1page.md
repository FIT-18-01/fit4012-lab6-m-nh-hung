# Report 1 page - Lab 6 AES-CBC Socket

## Thông tin nhóm

- Thành viên 1: Bùi Đình Mạnh
- Thành viên 2: Phạm Ngọc Hùng

## Mục tiêu

Bài lab nhằm xây dựng hệ thống gửi và nhận dữ liệu mã hóa bằng AES-CBC thông qua socket TCP. Hệ thống gồm hai thành phần chính là Sender và Receiver, trong đó Sender thực hiện mã hóa dữ liệu còn Receiver thực hiện giải mã dữ liệu nhận được. Bài lab sử dụng hai kênh riêng biệt gồm key channel để truyền AES key và IV, và data channel để truyền ciphertext. Ngoài ra, bài lab còn yêu cầu kiểm thử hệ thống bằng pytest, ghi log minh chứng, phân tích threat model và chỉ ra các điểm yếu bảo mật của thiết kế hiện tại.

## Phân công thực hiện

- Thành viên 1 phụ trách chính sender.py, AES encryption, key channel, sender log và kiểm tra packet gửi.
- Thành viên 2 phụ trách chính receiver.py, AES decryption, data channel, receiver log và xử lý nhận dữ liệu.
- Cả hai cùng thực hiện README.md, report-1page.md, threat-model-1page.md, peer-review-response.md, pytest, demo và kiểm tra toàn bộ hệ thống.

## Cách làm

Receiver được chạy trước để lắng nghe trên KEY_PORT và DATA_PORT. Sender đọc dữ liệu từ MESSAGE hoặc INPUT_FILE, sau đó sinh AES key và IV rồi thực hiện mã hóa bằng AES-CBC. Trước khi mã hóa, plaintext được padding bằng PKCS#7 để đảm bảo độ dài là bội số của 16 byte. Sender gửi AES key và IV qua key channel theo định dạng [key_length][key][iv], sau đó gửi ciphertext qua data channel theo định dạng [ciphertext_length][ciphertext]. Receiver sử dụng recv_exact() để nhận đủ dữ liệu, parse packet, giải mã ciphertext bằng AES-CBC và thực hiện unpadding để khôi phục plaintext ban đầu.

## Kết quả

Hệ thống chạy thành công ở cả hai chế độ gửi MESSAGE và gửi dữ liệu từ INPUT_FILE. Receiver nhận đúng AES key, IV và ciphertext từ Sender, sau đó giải mã thành công và khôi phục plaintext giống với dữ liệu đầu vào ban đầu. Các log minh chứng được lưu trong thư mục logs/, bao gồm sender_success.log và receiver_success.log. File sample_output.txt khớp hoàn toàn với sample_input.txt. Hệ thống cũng vượt qua các test quan trọng như happy path, key channel contract, data channel contract, wrong key, tamper ciphertext và invalid packet bằng pytest.

## Kết luận

Qua bài lab này, nhóm hiểu rõ hơn về socket TCP, AES-CBC, PKCS#7 padding, ciphertext, key management và packet structure. Nhóm cũng nhận thấy rằng việc gửi AES key plaintext qua socket là không an toàn trong hệ thống thực tế. Bài lab giúp hiểu được tầm quan trọng của key management, authentication, secure channel và threat model trong các hệ thống bảo mật ứng dụng thực tế.
