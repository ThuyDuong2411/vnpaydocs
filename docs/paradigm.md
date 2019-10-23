## Mô hình kết nối
<p>
    <img src="/img/mohinh.png">
</p>

* Bước 1: Khách hàng thực hiện mua hàng trên Website TMĐT và Thanh toán trực tuyến cho Đơn hàng.
* Bước 2: Website TMĐT thành lập yêu cầu thanh toán dưới dạng URL mang thông tin thanh toán và chuyển hướng khách hàng sang Cổng thanh toán VNPAY bằng URL đó.
    Cổng thanh toán VNPAY xử lý yêu cầu Thanh toán mà Website TMĐT gửi sang. Khách hàng tiến hành nhập thông tin được yêu cầu để thực hiện việc thanh toán.
* Bước 3,4: Khách hàng nhập thông tin để xác minh tài khoản Ngân hàng của khách hàng và xác thực giao dịch.
* Bước 5: Giao dịch thành công tại Ngân hàng, VNPAY tiến hành: 
    - Chuyển hướng khách hàng về Website TMĐT (vnp_ReturnUrl)
    - Thông báo cho Website TMĐT kết quả thanh toán của khách hàng thông qya IPN URL. Merchant cập nhật kết quả thanh toán VNPAY gửi tại URL này.
* Bước 6: Merchant hiển thị kết quả giao dịch tới khách hàng.