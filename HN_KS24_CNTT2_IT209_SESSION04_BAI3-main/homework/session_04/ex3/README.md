# Báo cáo Thực hành - Bài 3: Cấu hình xác thực SSH và Đẩy dự án lên GitHub

---

## 1. Thông tin sinh viên & Dự án
- **Họ và tên:** Ngô Trung Chiến
- **Lớp:** HN_KS24_CNTT2
- **Mã sinh viên / Tài khoản GitHub:** [Ngotrungchien232](https://github.com/Ngotrungchien232)
- **Môn học:** IT209 - DevOps
- **Bài tập:** Bài 3 - Cấu hình xác thực SSH và Đẩy dự án lên GitHub
- **Đường dẫn Repository (GitHub Web):** [https://github.com/Ngotrungchien232/HN_KS24_CNTT2_IT209_SESSION04_BAI3](https://github.com/Ngotrungchien232/HN_KS24_CNTT2_IT209_SESSION04_BAI3)
- **Đường dẫn Repository (SSH):** `git@github.com:Ngotrungchien232/HN_KS24_CNTT2_IT209_SESSION04_BAI3.git`
- **Đường dẫn nộp bài:** `homework/session_04/ex3/`

---

## 2. Mục tiêu bài thực hành
1. **Khởi tạo cặp khóa SSH bảo mật:** Sinh cặp khóa SSH sử dụng thuật toán **Ed25519** phục vụ mục đích xác thực kết nối từ xa.
2. **Cấu hình Public Key trên GitHub:** Đăng ký khóa công khai với dịch vụ GitHub để ủy quyền truy cập máy chủ Git an toàn.
3. **Cấu hình liên kết Remote Repository:** Cấu hình liên kết an toàn giữa repository cục bộ và máy chủ GitHub bằng giao thức **SSH** (tuyệt đối không sử dụng HTTPS token).
4. **Kiểm tra và xác thực kết nối:** Kiểm tra kết nối danh tính thông qua `ssh -T git@github.com` và cấu hình `git remote -v`.
5. **Đẩy (Push) mã nguồn:** Đẩy thành công mã nguồn và lịch sử commit lên GitHub qua kênh mã hóa SSH.

---

## 3. Các bước thực hiện chi tiết

### Bước 1: Khởi tạo cặp khóa SSH với thuật toán Ed25519

Chạy lệnh sinh cặp khóa SSH với thuật toán `Ed25519` cùng email định danh:

```bash
ssh-keygen -t ed25519 -C "ngotrungchien232@gmail.com"
```

> **Tại sao chọn Ed25519 thay vì RSA?**
> - **Độ an toàn cao hơn:** Ed25519 dựa trên đường cong Elliptic (Curve25519), cung cấp mức độ bảo mật tương đương RSA 3072-bit nhưng với kích thước khóa chỉ 256-bit.
> - **Tốc độ nhanh và kích thước gọn:** Sinh khóa, ký số và xác thực nhanh hơn rất nhiều so với RSA.
> - **Khả năng chống tấn công:** Miễn nhiễm với các kỹ thuật tấn công kênh phụ (side-channel/timing attacks).
> - **Khuyến nghị chuẩn:** Được GitHub và cộng đồng DevOps khuyến khích sử dụng mặc định hiện nay.

Quá trình sinh khóa tạo ra 2 tệp:
- `~/.ssh/id_ed25519` : **Khóa bí mật (Private Key)** - Được lưu trữ an toàn trên máy cục bộ, **tuyệt đối không chia sẻ hoặc tải lên kho mã nguồn**.
- `~/.ssh/id_ed25519.pub` : **Khóa công khai (Public Key)** - Dùng để đăng ký lên GitHub.

---

### Bước 2: Thêm SSH Public Key vào tài khoản GitHub

1. Xem và sao chép nội dung khóa công khai:
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
   *Nội dung khóa public:*
   ```text
   ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIt96kfWxCwIqzRdz7mwqjqUnZpqRHsuXnKiED1ZtCxQ email_github_cua_ban@example.com
   ```

2. Đăng ký trên GitHub:
   - Truy cập GitHub -> **Settings** -> **SSH and GPG keys** (hoặc truy cập trực tiếp `https://github.com/settings/ssh/new`).
   - Nhấn **New SSH key**.
   - **Title:** `Azure-DevOps-Ed25519` (hoặc tên máy làm việc).
   - **Key type:** `Authentication Key`.
   - **Key:** Dán toàn bộ nội dung khóa `id_ed25519.pub` vào ô này.
   - Nhấn **Add SSH key**.

---

### Bước 3: Kiểm tra kết nối SSH tới GitHub

Thực hiện lệnh kiểm tra xác thực danh tính với máy chủ GitHub:

```bash
ssh -T git@github.com
```

**Kết quả thực tế:**
```text
Hi Ngotrungchien232! You've successfully authenticated, but GitHub does not provide shell access.
```

**Phân tích kết quả:**
- Thông báo `Hi Ngotrungchien232! You've successfully authenticated` xác nhận GitHub đã nhận diện và xác thực thành công người dùng `Ngotrungchien232` thông qua cặp khóa Ed25519.
- Thông báo `GitHub does not provide shell access` là hành vi chuẩn và an toàn của GitHub, vì GitHub chỉ cấp quyền thao tác Git qua giao thức SSH chứ không cấp interactive login shell trên server của họ.

---

### Bước 4: Khởi tạo Local Repository và Cấu hình Remote SSH

1. Khởi tạo repository cục bộ:
   ```bash
   git init
   git branch -M main
   ```

2. Cấu hình liên kết Remote Origin sử dụng URL giao thức SSH:
   ```bash
   git remote add origin git@github.com:Ngotrungchien232/HN_KS24_CNTT2_IT209_SESSION04_BAI3.git
   ```

---

### Bước 5: Kiểm tra cấu hình Remote URL

Thực hiện lệnh kiểm tra danh sách remote URL:

```bash
git remote -v
```

**Kết quả thực tế:**
```text
origin	git@github.com:Ngotrungchien232/HN_KS24_CNTT2_IT209_SESSION04_BAI3.git (fetch)
origin	git@github.com:Ngotrungchien232/HN_KS24_CNTT2_IT209_SESSION04_BAI3.git (push)
```

**Đánh giá:**
- Cả hai luồng `fetch` và `push` đều trỏ chính xác về định dạng SSH: `git@github.com:username/repository.git`.
- Thỏa mãn 100% ràng buộc của bài toán: không sử dụng giao thức HTTPS.

---

### Bước 6: Đẩy (Push) mã nguồn lên GitHub bằng giao thức SSH

1. Thêm toàn bộ tệp vào Staging Area và tạo commit:
   ```bash
   git add .
   git commit -m "feat: complete session 04 exercise 3 - ssh authentication & push to github"
   ```

2. Đẩy mã nguồn lên GitHub:
   ```bash
   git push -u origin main
   ```

**Kết quả thực tế:**
```text
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 16 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (7/7), done.
Total 7 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:Ngotrungchien232/HN_KS24_CNTT2_IT209_SESSION04_BAI3.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

Quá trình đẩy diễn ra hoàn toàn tự động thông qua xác thực khóa bí mật Ed25519, không yêu cầu nhập mật khẩu hay Personal Access Token (PAT).

---

## 4. Bảng tổng kết lệnh và kết quả kiểm tra

| Tiêu chí | Lệnh thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái |
| :--- | :--- | :--- | :--- | :---: |
| **Kiểm tra SSH** | `ssh -T git@github.com` | Xác nhận tài khoản `Ngotrungchien232` thành công | `Hi Ngotrungchien232! You've successfully authenticated...` | **PASSED** |
| **Kiểm tra Remote** | `git remote -v` | URL dạng `git@github.com:username/repository.git` | `origin git@github.com:Ngotrungchien232/HN_KS24_CNTT2_IT209_SESSION04_BAI3.git` | **PASSED** |
| **Đẩy mã nguồn** | `git push -u origin main` | Push thành công branch `main` lên GitHub | `branch 'main' set up to track 'origin/main'` | **PASSED** |

---

## 5. Nguyên tắc an ninh bảo mật DevOps
1. **Bảo mật Private Key:** Khóa riêng tư (`id_ed25519`) được lưu trữ tại thư mục cá nhân với phân quyền chặt chẽ (`chmod 600`), không bao giờ chia sẻ hay commit lên repository.
2. **File `.gitignore`:** Cấu hình loại trừ các tệp nhạy cảm (khóa riêng tư, mật khẩu, file môi trường `.env`).
3. **Phân biệt Public Key vs Private Key:**
   - **Private Key:** Giữ trên máy client, dùng để tạo chữ ký số mã hóa.
   - **Public Key:** Đưa lên server (GitHub), dùng để giải mã và xác thực chữ ký của client.
