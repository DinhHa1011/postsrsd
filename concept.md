- SRS là một kỹ thuật chuyển tiếp thư từ các miền triển khai khung chính sách người gửi (SPF) để cấm MTA khác gửi thư thay mặt họ
- Với SRS, một MTA có thể phá vỡ các hạn chế SPF bằng cách thay thế người gửi phong bì bằng 1 địa chỉ email tạm thời từ một trong các miền của riêng họ. Địa chỉ tạm thời này được liên kết với người gửi ban đầu và chỉ có hiệu lực trong một ktg nhất định, điều này ngăn chặn sự lạm dụng của những kẻ gửi thư rác
- Postsrsd: thực hiện sơ đồ viết lại người gửi (srs) cho postfix
- Lược đồ viết lại của người gửi (srs) là bắt buộc để phù hợp với lược đồ spf khi email được chuyển tiếp 
- Sender Rewriting scheme (srs) là một process viết lại địa chỉ người gửi từ
```
sender@example.com
```
thành
```
SRSO+XXXX+ZZ+example.com+sender@yourdomain.ldp
```
