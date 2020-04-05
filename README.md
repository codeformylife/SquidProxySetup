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
