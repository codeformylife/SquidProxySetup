==================== Đổi pass VPS ====================
1. Thực thi quyền root: sudo su
2. Đổi mật khẩu root: sudo passwd
3. Cho phép SSH sử dụng phương thức mật khẩu
        - Sửa file: vi /etc/ssh/sshd_config
        - Sửa Permission root login: no thành yes
        - Sửa Password authentication : no thành yes
4. Khởi động lại dịch vụ sshd: service sshd restart
5. cài bitvise client: https://www.bitvise.com/ssh-client-do...
6. Login và tận hưởng thành quả.

==================== Cài đặt Squid HTTP Proxy Server ====================
0. sudo yum -y update
1. sudo yum -y install squid
2. sudo systemctl start squid
3. sudo systemctl enable squid
4. yum -y install httpd-tools
5. touch /etc/squid/passwd
6. chown squid: /etc/squid/passwd
7. htpasswd /etc/squid/passwd luca
8. add ACL (vim /etc/squid/squid.conf)
        auth_param basic program /usr/lib64/squid/basic_ncsa_auth /etc/squid/passwd
        auth_param basic children 5
        auth_param basic realm Squid Basic Authentication
        auth_param basic credentialsttl 2 hours
        acl auth_users proxy_auth REQUIRED
        http_access allow auth_users
9. systemctl restart squid

#Check status(active is ok) : sudo systemctl status squid

#google cloud
10. Tạo firewall rule để chấp nhận port 3218
        Bấm vào Firewall rules phía bên trái -> Create Firewall Rule
        Điền các trường như sau rồi bấm Create:
                 direction of trafic : ingress
                 action on mactch : allow
                 target : all instances in the network
                 source ip: 0.0.0.0/0
                 protocals and ports : tcp 3128

#Cấu hình Squid như 1 HTTP Proxy Server
        Mặc định Squid sẽ chạy mặc định trên cổng 3218, chúng ta có thể thay đổi cổng này, hoặc thêm cổng kết nối khác bằng việc thêm dòng sau (VD: port 8080 ) vào file /etc/squid/squid.conf
        ->>  http_port 8080
        Sau đó restart lại squid
        ->> sudo systemctl restart squid

#Mở cổng (port) trên CentOS 7/8
        Tưởng lửa trên CentOS 7/8 giờ được quản lý bằng công cụ firewall-cmd, nên để mở port sử dụng command sau với quyền của tài khoản root.
        – Kiểm tra zone nào của tường lửa đang được active
        ->> firewall-cmd --get-active-zones
        – Mở cổng (VD: 80) trên zone đang active (Public Zone)
        ->> firewall-cmd --zone=public --add-port=80/tcp --permanent
        – Sau đó để luật mới có hiệu lực cần reload lại tường lửa bằng command sau:
        ->> firewall-cmd --reload