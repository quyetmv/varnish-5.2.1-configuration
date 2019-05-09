# varnish-5.2.1-configuration
Cấu hình mẫu cho Varnish 5.2.1

Môi trường:
CentOS Linux release 7.6.1810 (Core)
Kernel: 3.10.0-957.12.1

Cấu hình:

2 x 2.6 Ghz CPU
3GB RAM

Thiết lập Server:
+ Nginx trên cổng 80 và 443 - Static và xử lý TLS/SSL
+ Varnish trên cổng 82 - Cache
+ Apache trên cổng 8181 - Backend vì cần sử dụng .htaccess và nhiều thứ khác (quen thuộc hơn)
+ PHP-FPM - Xử lý về PHP, cũng như Opcache ...

Cấu hình này được tách thành rất nhiều file nhỏ tùy theo mỗi website khác nhau. Nếu không có nhu cầu có thể gộp lại

Restart/ Reload lại Varnish sau khi thực hiện thay đổi và đảm bảo Varnish chạy trước khi test
Để kiểm tra sau varnish hoạt động (có HIT) hay không, kiểm tra header xem:

Age: 0
X-Cache: MISS
X-Cache-Hits: 0

Có chuyển thành 

Age: [x]
X-Cache: HIT
X-Cache-Hits: 1

Sau mỗi lần request và số hit có tăng thêm không

Rất hoan nghênh góp ý từ bạn nếu thấy răng cầu hình này lỗi thời/ sai/ chưa tối ưu
