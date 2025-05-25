Hướng Dẫn Cài Đặt và Cấu Hình reNgine và DVWA
Hướng dẫn này cung cấp các bước chi tiết để cài đặt và cấu hình reNgine (một công cụ quét bảo mật) và DVWA (Damn Vulnerable Web Application) trên máy ảo Ubuntu sử dụng VMware.

Cài Đặt reNgine
Bước 1: Chuẩn Bị Môi Trường

Máy ảo: Sử dụng máy ảo Ubuntu trên VMware với cấu hình:
CPU: 6 nhân
RAM: 6GB


Lưu ý: Nên sử dụng máy có cấu hình cao để đảm bảo hiệu suất tối ưu trong môi trường thực tế.
Cập nhật hệ thống:sudo apt update && sudo apt upgrade -y



Bước 2: Tải Source Code reNgine

Clone mã nguồn từ GitHub:git clone https://github.com/yogeshojha/reNgine



Bước 3: Cấu Hình reNgine

Di chuyển vào thư mục reNgine:
cd reNgine


Chỉnh sửa file .env:
vi .env


Giải thích các biến môi trường trong file .env:



Mục
Biến
Giá trị mẫu
Mô tả



General
COMPOSE_PROJECT_NAME
reNgine
Tên dự án dùng trong Docker Compose để định danh container, network, volume.


SSL Specific Configuration






AUTHORITY_NAME
reNgine
Tên tổ chức phát hành chứng chỉ SSL (Certificate Authority).



AUTHORITY_PASSWORD
nSrmNkwT
Mật khẩu bảo vệ private key của CA.



COMPANY
reNgine
Tên công ty/tổ chức trong metadata chứng chỉ SSL.



DOMAIN_NAME
reNgine.example.com
Tên miền áp dụng chứng chỉ SSL (thay bằng domain thực tế).



COUNTRY_CODE
US
Mã quốc gia (ISO 3166-1 alpha-2, ví dụ: VN cho Việt Nam).



STATE
Georgia
Tên bang/tỉnh trong metadata chứng chỉ.



CITY
Atlanta
Tên thành phố trong metadata chứng chỉ.


Database Configurations






POSTGRES_DB
reNgine
Tên cơ sở dữ liệu chính của reNgine.



POSTGRES_USER
reNgine
Tên người dùng đăng nhập vào cơ sở dữ liệu.



POSTGRES_PASSWORD
hE2a5@K&9nEY1fzgA6X
Mật khẩu của người dùng cơ sở dữ liệu.



POSTGRES_PORT
5432
Cổng mặc định của PostgreSQL.



POSTGRES_HOST
db
Tên host/container chứa cơ sở dữ liệu (thường là tên service trong docker-compose.yml).


Celery Scaling Configurations






MAX_CONCURRENCY
80
Số lượng worker Celery tối đa chạy song song.



MIN_CONCURRENCY
10
Số lượng worker tối thiểu để tiết kiệm tài nguyên.


reNgine Web Interface Super User






DJANGO_SUPERUSER_USERNAME
reNgine
Tên đăng nhập của tài khoản quản trị viên.



DJANGO_SUPERUSER_EMAIL
reNgine@example.com
Email liên kết với tài khoản quản trị.



DJANGO_SUPERUSER_PASSWORD
Sm7IJG.IfHAFw9snSKv
Mật khẩu của tài khoản quản trị.



Lưu ý về cấu hình MAX_CONCURRENCY và MIN_CONCURRENCY:

Gợi ý dựa trên RAM:
4GB: MAX_CONCURRENCY=10
8GB: MAX_CONCURRENCY=30
16GB: MAX_CONCURRENCY=50


Trong demo này, với cấu hình 6GB RAM:
MAX_CONCURRENCY=30
MIN_CONCURRENCY=10


Lưu ý: Mặc dù không đúng với gợi ý chính thức, công cụ vẫn hoạt động bình thường.



Bước 4: Cài Đặt reNgine

Chạy script cài đặt:
sudo ./install.sh


Lưu ý: reNgine sử dụng Docker để triển khai, tận dụng container hóa để đảm bảo tính nhất quán và dễ dàng di chuyển.

Xử lý lỗi liên quan đến createsuperuser (nếu xảy ra):
sudo docker exec -it reNgine-web-1 python3 manage.py migrate
sudo docker exec -it reNgine-web-1 python3 manage.py createsuperuser
sudo docker compose down
sudo docker compose -f docker-compose.setup.yml up --build
sudo docker compose up -d

Giải thích:

migrate: Chạy các migration để đồng bộ cơ sở dữ liệu với Django.
createsuperuser: Tạo tài khoản quản trị viên thủ công.
docker compose down: Dừng và gỡ các container.
docker-compose.setup.yml up --build: Build lại image từ Dockerfile.
docker compose up -d: Khởi chạy các dịch vụ ở chế độ nền.



Bước 5: Kiểm Tra và Cấu Hình Sau Cài Đặt

Kiểm tra các container đang chạy:
sudo docker ps


Kết quả: Các container chạy trên port 443 (HTTPS).
Hình ảnh tham khảo: ./assets/Picture4.png


Truy cập giao diện web:

Địa chỉ: https://192.168.1.26
Hình ảnh tham khảo: ./assets/Picture5.png


Đăng nhập:

Sử dụng tài khoản đã thiết lập trong .env, ví dụ:
Username: root
Password: 123456


Hình ảnh tham khảo: ./assets/Picture6.png, ./assets/Picture7.png, ./assets/Picture8.png


Cấu hình API Key:

Truy cập các dịch vụ sau để lấy API key:
OpenAI: https://platform.openai.com/settings/organization/api-keys
Netlas: https://app.netlas.io/profile/
Chaos: https://cloud.projectdiscovery.io
HackerOne: https://hackerone.com/settings/api_token/edit


Giao diện sau khi hoàn tất cấu hình: ./assets/Picture9.png




Cài Đặt DVWA
Bước 1: Tải Mã Nguồn DVWA

Clone mã nguồn từ GitHub:git clone https://github.com/digininja/DVWA.git


Hình ảnh tham khảo: ./assets/Picture10.png



Bước 2: Di Chuyển Thư Mục DVWA

Di chuyển thư mục DVWA vào /var/www/html:mv DVWA/ /var/www/html/dvwa


Hình ảnh tham khảo: ./assets/Picture11.png



Bước 3: Cấu Hình DVWA

Chỉnh sửa file cấu hình:
File: /var/www/html/dvwa/config/config.inc.php
Nội dung cấu hình:


Biến
Giá trị mặc định
Mô tả



db_server
127.0.0.1
Địa chỉ host của database server (localhost).


db_database
dvwa
Tên cơ sở dữ liệu.


db_user
admin
Tên người dùng database.


db_password
password
Mật khẩu database.


db_port
3306
Cổng kết nối MySQL.



Hình ảnh tham khảo: ./assets/Picture12.png



Bước 4: Cấp Quyền Cho Thư Mục DVWA

Cấp quyền đầy đủ cho thư mục:sudo chmod -R 777 /var/www/html/dvwa


Hình ảnh tham khảo: ./assets/Picture13.png



Bước 5: Tạo Cơ Sở Dữ Liệu

Thực hiện các lệnh sau để tạo database:
sudo su
mysql -u root -p
create database dvwa;
create user 'admin'@'127.0.0.1' identified by 'password';
grant all privileges on dvwa.* to 'admin'@'127.0.0.1';
exit

Giải thích:

sudo su: Chuyển sang người dùng root.
mysql -u root -p: Đăng nhập vào MySQL.
create database dvwa: Tạo cơ sở dữ liệu dvwa.
create user: Tạo user admin với mật khẩu password.
grant all privileges: Cấp toàn quyền cho user admin trên database dvwa.
exit: Thoát khỏi MySQL.



Bước 6: Cấu Hình Apache

Khởi động Apache:
sudo systemctl start apache2


Chỉnh sửa file php.ini:
sudo vi /etc/php/8.2/apache2/php.ini


Tìm và sửa: allow_url_include = On
Lưu và thoát.


Khởi động lại Apache:
sudo systemctl restart apache2.service



Bước 7: Cấu Hình Ban Đầu DVWA

Truy cập: http://127.0.0.1/dvwa/setup.php
Nhấn Create/Reset Database để tạo hoặc reset cơ sở dữ liệu.
Hình ảnh tham khảo: ./assets/Picture14.png

Bước 8: Kiểm Tra Kết Quả

Truy cập: http://127.0.0.1/dvwa/login.php
Đăng nhập:
Username: admin
Password: password


Giao diện sau khi đăng nhập: ./assets/Picture15.png


