## Hướng dẫn tích hợp

Tạo tài khoản merchant môi trường test, VNPAY sẽ gửi thông tin cấu hình cho website của bạn gồm mã website (vnp_TmnCode) và chuỗi bí mật tạo checksum (vnp_HashSecret).

Hướng dẫn tích hợp Cổng thanh toán VNPAY đã có chi tiết trên [website của VNPAY](https://sandbox.vnpayment.vn/apis/docs/huong-dan-tich-hop/)

Demo hướng dẫn:

* Bước 1: Đăng ký account merchant tạo môi trường test, VNPAY sẽ cũng cấp thông tin cấu hình như sau:

<p>
        <img src="/img/cauhinh.png">
</p>

* Bước 2: Tạo URL thanh toán.

Website TMĐT gửi sang Cổng thanh toán VNPAY các thông tin này khi xử lý giao dịch thanh toán trực tuyến cho khách mua hàng.

URL có dạng:

        http://sandbox.vnpayment.vn/paymentv2/vpcpay.html?vnp_Amount=51750000&vnp_BankCode=NCB&vnp_Command=pay&vnp_CreateDate=20191023161955&vnp_CurrCode=VND&vnp_IpAddr=127.0.0.1&vnp_Locale=vn&vnp_OrderInfo=Thanh+to%C3%A1n+%C4%91%C6%A1n+h%C3%A0ng&vnp_OrderType=billpayment&vnp_ReturnUrl=http%3A%2F%2Fali33.ga%2Flist-cart&vnp_TmnCode=WDMUV7FB&vnp_TxnRef=40&vnp_Version=2.0.0&vnp_SecureHashType=SHA256&vnp_SecureHash=c4024fd86df5435d7f5b26988bd510f44830be92222d82afc60eee92cc1fcf58

`vnp_Amount`: Số tiền khách hàng cần thanh toán.

`vnp_BankCode`: Mã ngân hàng thanh toán.

`vnp_Command`: Mã API sử dụng, mã cho giao dịch thanh toán là pay.

`vnp_CreateDate`: Ngày thanh toán giao dịch.

`vnp_CurCode`: Đơn vị tiền tệ sử dụng thanh toán. Hiện tại chỉ hỗ trợ VND.

`vnp_IpAddr`: Địa chỉ IP của khách hàng thực hiện giao dịch.

`vnp_Locale`: Ngôn ngữ giao diện hiển thị. Hiện tại hỗ trợ Tiếng Việt (vn), Tiếng Anh (en).

`vnp_OrderInfo`: Thông tin mô tả nội dung thanh toán (Tiếng Việt, không dấu).

`vnp_OrderType`: Mã danh mục hàng hóa (có thể tùy chọn).

`vnp_ReturnUrl`: URL thông báo kết quả giao dịch khi khách hàng kết thúc thanh toán.

`vnp_TmnCode`: Mã website của merchant trên hệ thống của VNPAY (đã được VNPAY cung cấp).

`vnp_TxnRef`: Mã hóa đơn cần thanh toán do merchant cung cấp, là duy nhất để phân biệt các đơn hàng gửi sang VNPAY.

`vnp_Version`: Phiên bản api mà merchant kết nối. Phiên bản hiện tại là 2.0.0

`vnp_SecureHashType`: Loại mã băm sử dụng. Ví dụ: SHA256, MD5.

`vnp_SecureHash`: Mã kiểm tra (checksum) để đảm bảo dữ liệu của giao dịch không bị thay đổi từ quá trình chuyển từ merchant sang VNPAY.

**Một số lưu ý của VNPAY**

1. Dữ liệu checksum được thành lập dựa trên việc sắp xếp tăng dần của tên tham số (QueryString).
2. Số tiền cần thanh toán nhân với 100 để triệt tiêu phần thập phân trước khi gửi sang VNPAY.
3. `vnp_BankCode`: Giá trị này tùy chọn:
    - Nếu bỏ trống, khách hàng sẽ chọn Ngân hàng thanh toán tại VNPAY.
    - Nếu thiết lập giá trị (chọn Ngân hàng thanh toán tại Website TMĐT), Tham khảo giá trị tại [Bảng mã Ngân hàng](https://sandbox.vnpayment.vn/apis/danh-sach-ngan-hang/).

* Bước 3: Tạo URL trả về (dùng để hiển thị màn hình thông báo kết quả giao dịch tới khách hàng, xử lý ở Frontend)

URL có dạng:

        http://ali33.ga/list-cart?vnp_Amount=51750000&vnp_BankCode=NCB&vnp_BankTranNo=20191023165545&vnp_CardType=ATM&vnp_OrderInfo=Thanh%20to%C3%A1n%20%C4%91%C6%A1n%20h%C3%A0ng&vnp_PayDate=20191023165534&vnp_ResponseCode=00&vnp_TmnCode=WDMUV7FB&vnp_TransactionNo=13185888&vnp_TxnRef=49&vnp_SecureHashType=SHA256&vnp_SecureHash=eb626884b217e028f44efe77f5c23716f96cd246c5b87ba7d4badacc9b0c07b6

**Một số lưu ý**

1. URL này hcir kiểm tra toàn vẹn dữ liệu (checksum) và hiển thị thông báo tới khách hàng.
2. Không cập nhật kết quả giao dịch tại địa chỉ này.

* Bước 4: Tạo URL IPN

Địa chỉ để nhận kết quả thanh toán từ VNPAY. Trên URL VNPAY gọi về có mang thông tin thanh toán để căn cứ vào kết quả đó Website TMĐT xử lý các bước tiếp theo.

URL có dạng:

        http://sandbox.vnpayment.vn/tryitnow/Home/VnPayIPN?vnp_Amount=1000000&vnp_BankCode=NCB&vnp_BankTranNo=20170829152730&vnp_CardType=ATM&vnp_OrderInfo=Thanh+toan+don+hang+thoi+gian%3A+2017-08-29+15%3A27%3A02&vnp_PayDate=20170829153052&vnp_ResponseCode=00&vnp_TmnCode=2QXUI4J4&vnp_TransactionNo=12996460&vnp_TxnRef=23597&vnp_SecureHashType=SHA256&vnp_SecureHash=20081f0ee1cc6b524e273b6d4050fefd

**Một số lưu ý**
