# HN_KS24_CNTT2_IT209_SESSION04_BAI3

## Bài 3: Cấu hình xác thực SSH và Đẩy dự án lên GitHub

- **Học viên:** Ngô Trung Chiến
- **Lớp:** HN_KS24_CNTT2
- **Tài khoản GitHub:** [Ngotrungchien232](https://github.com/Ngotrungchien232)
- **Môn học:** IT209 - DevOps

### Cấu trúc thư mục bài nộp
Báo cáo chi tiết quá trình tạo khóa SSH Ed25519, liên kết remote repository và đẩy mã nguồn lên GitHub được lưu tại:
📂 **[homework/session_04/ex3/README.md](homework/session_04/ex3/README.md)**

### Tóm tắt kết quả kiểm tra
1. **Kiểm tra kết nối SSH:**
   ```bash
   ssh -T git@github.com
   ```
   *Kết quả:* `Hi Ngotrungchien232! You've successfully authenticated, but GitHub does not provide shell access.`

2. **Kiểm tra Remote URL:**
   ```bash
   git remote -v
   ```
   *Kết quả:*
   ```text
   origin  git@github.com:Ngotrungchien232/HN_KS24_CNTT2_IT209_SESSION04_BAI3.git (fetch)
   origin  git@github.com:Ngotrungchien232/HN_KS24_CNTT2_IT209_SESSION04_BAI3.git (push)
   ```

3. **Giao thức sử dụng:** Hoàn toàn bằng giao thức SSH và thuật toán Ed25519 (không sử dụng HTTPS token).
