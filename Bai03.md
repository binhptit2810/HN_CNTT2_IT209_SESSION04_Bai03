# Bài 3: Cấu hình xác thực SSH và Đẩy dự án lên GitHub

## 1. Mục tiêu

- Tạo cặp khóa SSH sử dụng thuật toán Ed25519.
- Cấu hình SSH để xác thực với GitHub.
- Liên kết Git Repository local với GitHub bằng giao thức SSH.
- Đẩy mã nguồn và lịch sử commit lên GitHub.
- Kiểm tra kết nối SSH và remote repository.

---

## 2. Kiểm tra Git

Kiểm tra phiên bản Git:

```bash
git --version
```

Git đã được cài đặt và sẵn sàng sử dụng.

---

## 3. Tạo SSH Key sử dụng Ed25519

Sử dụng lệnh:

```bash
ssh-keygen -t ed25519 -C "EMAIL_GITHUB"
```

Trong đó:

- `-t ed25519`: sử dụng thuật toán Ed25519.
- `-C`: thêm email GitHub làm comment cho SSH key.

Ví dụ:

```bash
ssh-keygen -t ed25519 -C "email@example.com"
```

Khi được hỏi vị trí lưu khóa, sử dụng vị trí mặc định:

```text
C:\Users\<username>\.ssh\id_ed25519
```

SSH tạo ra hai file:

```text
id_ed25519
id_ed25519.pub
```

Trong đó:

- `id_ed25519`: Private Key, phải được giữ bí mật.
- `id_ed25519.pub`: Public Key, có thể cung cấp cho GitHub.

**Không đưa file `id_ed25519` lên GitHub.**

---

## 4. Kiểm tra SSH Key

Kiểm tra thư mục `.ssh`:

```bash
ls ~/.ssh
```

Trên Windows PowerShell có thể sử dụng:

```powershell
Get-ChildItem $env:USERPROFILE\.ssh
```

Kết quả có thể bao gồm:

```text
id_ed25519
id_ed25519.pub
```

---

## 5. Lấy Public Key

Hiển thị Public Key:

```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub
```

Kết quả có dạng:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... email@example.com
```

Chỉ sử dụng nội dung của file:

```text
id_ed25519.pub
```

để thêm vào GitHub.

**Không chia sẻ nội dung của `id_ed25519`.**

---

## 6. Thêm SSH Key vào GitHub

Đăng nhập GitHub.

Vào:

```text
Settings
→ SSH and GPG keys
→ New SSH key
```

Đặt tên cho key, ví dụ:

```text
My Windows 11 Laptop
```

Chọn loại:

```text
Authentication Key
```

Dán nội dung của:

```text
id_ed25519.pub
```

vào ô SSH Key.

Sau đó chọn:

```text
Add SSH key
```

SSH Public Key đã được liên kết với tài khoản GitHub.

---

## 7. Kiểm tra kết nối SSH tới GitHub

Chạy:

```bash
ssh -T git@github.com
```

Lần đầu kết nối có thể xuất hiện thông báo xác nhận fingerprint.

Chọn:

```text
yes
```

Kết quả mong đợi:

```text
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

Điều này chứng minh SSH Key đã xác thực thành công với GitHub.

---

## 8. Tạo GitHub Repository

Tạo một repository mới trên GitHub.

Ví dụ:

```text
Repository name: git-session-04
```

Repository có thể để Public hoặc Private tùy yêu cầu.

Không cần tạo thêm README trên GitHub nếu Repository local đã có README và commit.

---

## 9. Liên kết Local Repository với GitHub bằng SSH

Trong thư mục project local:

```bash
git remote add origin git@github.com:USERNAME/REPOSITORY.git
```

Ví dụ:

```bash
git remote add origin git@github.com:binhptit2810/git-session-04.git
```

Lưu ý remote phải sử dụng:

```text
git@github.com:
```

chứ không phải:

```text
https://github.com/
```

---

## 10. Kiểm tra Remote URL

Chạy:

```bash
git remote -v
```

Kết quả mong đợi:

```text
origin  git@github.com:USERNAME/REPOSITORY.git (fetch)
origin  git@github.com:USERNAME/REPOSITORY.git (push)
```

Ví dụ:

```text
origin  git@github.com:binhptit2810/git-session-04.git (fetch)
origin  git@github.com:binhptit2810/git-session-04.git (push)
```

Remote `origin` đang sử dụng giao thức SSH.

---

## 11. Kiểm tra branch

Kiểm tra branch hiện tại:

```bash
git branch
```

Nếu sử dụng `main`:

```text
* main
```

Nếu cần đổi tên branch hiện tại thành `main`:

```bash
git branch -M main
```

---

## 12. Push dự án lên GitHub

Đẩy branch `main` lên GitHub:

```bash
git push -u origin main
```

Nếu cấu hình SSH chính xác, Git sẽ kết nối với GitHub mà không yêu cầu nhập password/token GitHub.

Kết quả thành công có dạng:

```text
Enumerating objects: ...
Counting objects: ...
Writing objects: ...
To github.com:USERNAME/REPOSITORY.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

---

## 13. Kiểm tra lại Remote

Chạy:

```bash
git remote -v
```

Kết quả:

```text
origin  git@github.com:USERNAME/REPOSITORY.git (fetch)
origin  git@github.com:USERNAME/REPOSITORY.git (push)
```

---

## 14. Kiểm tra lịch sử commit

Chạy:

```bash
git log --oneline
```

Ví dụ:

```text
a1b2c3d Initial commit
```

Lịch sử commit local đã được đẩy lên GitHub cùng với mã nguồn.

---

## 15. Thông tin Repository

GitHub Repository:

```text
https://github.com/USERNAME/REPOSITORY
```

SSH Remote:

```text
git@github.com:USERNAME/REPOSITORY.git
```

Ví dụ:

```text
https://github.com/binhptit2810/git-session-04
```

SSH:

```text
git@github.com:binhptit2810/git-session-04.git
```

---

## 16. Kết quả kiểm tra

### Kiểm tra SSH

Lệnh:

```bash
ssh -T git@github.com
```

Kết quả:

```text
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

### Kiểm tra Remote

Lệnh:

```bash
git remote -v
```

Kết quả:

```text
origin  git@github.com:USERNAME/REPOSITORY.git (fetch)
origin  git@github.com:USERNAME/REPOSITORY.git (push)
```

### Kiểm tra lịch sử commit

Lệnh:

```bash
git log --oneline
```

Kết quả:

```text
a1b2c3d Initial commit
```

### Push

Lệnh:

```bash
git push -u origin main
```

Kết quả:

```text
main -> main
```

---

## 17. Lưu ý bảo mật

Private Key:

```text
id_ed25519
```

không được đưa lên GitHub hoặc chia sẻ cho người khác.

Public Key:

```text
id_ed25519.pub
```

được sử dụng để đăng ký với GitHub.

Không đưa các file sau vào repository:

```text
id_ed25519
id_ed25519.pub
```

Đặc biệt, Private Key phải luôn được bảo mật.

---

## 18. Kết luận

Đã hoàn thành cấu hình xác thực SSH sử dụng thuật toán Ed25519.

Các bước chính đã thực hiện:

1. Tạo SSH Key Ed25519.
2. Thêm Public Key vào tài khoản GitHub.
3. Kiểm tra kết nối bằng `ssh -T git@github.com`.
4. Cấu hình Remote sử dụng giao thức SSH.
5. Kiểm tra bằng `git remote -v`.
6. Push branch `main` lên GitHub bằng SSH.
7. Kiểm tra lịch sử commit sau khi push.

Repository đã được cấu hình sử dụng SSH thay vì HTTPS.
