Phần 1: Phân tích lỗi

## Do lệnh sử dụng cờ **\-CaseSensitive** bắt buộc khớp chính xác chữ hoa/thường, trong khi pattern nhập vào là "critical" (chữ thường) nhưng dữ liệu log thực tế lại ghi là CRITICAL (chữ hoa). Lệnh không tìm thấy dòng nào nên file bị trống.

Phần 2: Sửa lỗi và kiểm thử

## **1\. Script sửa đúng (bỏ \-CaseSensitive và đổi pattern thành "CRITICAL"):**

"--- Bat dau quat log: \$(Get-Date) \---" | Out-File daily\_critical.txt  
Select-String \-Path server.log \-Pattern "CRITICAL" | Out-File \-Append daily\_critical.txt  
"--- Hoan tat. So dong loi: \---" | Out-File \-Append daily\_critical.txt  
(Get-Content daily\_critical.txt).Count | Out-File \-Append daily\_critical.txt  
**2\. Kết quả mong đợi của file daily\_critical.txt:**  
\--- Bat dau quat log: \[Ngày\_Giờ\_Chạy\_Script\] \---  
server.log:2:2026-09-18 02:30:11 CRITICAL Database connection pool exhausted  
server.log:3:2026-09-18 02:30:45 CRITICAL Retry limit exceeded for payment-service  
\--- Hoan tat. So dong loi: \---  
4  
**3\. Nếu file server.log không tồn tại:**  
Script **sẽ báo lỗi đỏ** (ItemNotFoundException) trên màn hình terminal tại dòng lệnh Select-String. Tuy nhiên, script không dừng hoàn toàn mà vẫn chạy tiếp các dòng ghi file còn lại.