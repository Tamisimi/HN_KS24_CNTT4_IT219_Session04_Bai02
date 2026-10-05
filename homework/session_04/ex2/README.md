# PTIT DevOps — Session 04

Project sau khi **gộp nhánh** `feature-update` vào `main` và đã **giải quyết merge conflict** thủ công trên file này.

## Nội dung thống nhất (sau conflict)

- Dòng từ **main**: cập nhật mô tả dự án trên nhánh chính.
- Dòng từ **feature-update**: bổ sung tính năng mới từ nhánh phụ.
- Kết quả giữ **cả hai** thay đổi hợp lý, không còn `<<<<<<<` / `=======` / `>>>>>>>`.

**Main contribution:** Tài liệu chính thức của khóa DevOps PTIT.

**Feature contribution:** Thêm hướng dẫn quản lý nhánh Git và xử lý xung đột.

---

## Báo cáo — Các bước giải quyết xung đột thủ công

### 1. Tạo nhánh và gây conflict

```bash
git checkout main
echo "Dong tu main" > README.md
git add README.md && git commit -m "main: update README"

git checkout -b feature-update
echo "Dong tu feature-update" > README.md
git add README.md && git commit -m "feature: update README"

git checkout main
echo "Dong khac tu main" > README.md
git add README.md && git commit -m "main: change same line"
```

### 2. Merge (xung đột)

```bash
git merge feature-update
# CONFLICT (content): Merge conflict in README.md
```

File lúc conflict:

```text
<<<<<<< HEAD
Dong khac tu main
=======
Dong tu feature-update
>>>>>>> feature-update
```

### 3. Xử lý thủ công

1. Mở `README.md`
2. Xóa toàn bộ `<<<<<<< HEAD`, `=======`, `>>>>>>> feature-update`
3. Giữ / viết lại nội dung cuối cùng mong muốn (kết hợp cả hai phía)
4. Lưu file

```bash
git add README.md
git commit -m "merge: resolve conflict in README.md"
```

### 4. Kiểm tra đồ thị nhánh

```bash
git log --graph --oneline
```

Kỳ vọng: commit merge có **2 parent** (main + feature-update).

Ví dụ:

```text
*   a1b2c3d merge: resolve conflict in README.md
|\  
| * e4f5g6h feature: update README
* | i7j8k9l main: change same line
|/
* m0n1o2p main: update README
```

## Ghi chú 3-Way Merge

Git so sánh 3 phiên bản: **base** (tổ tiên chung), **HEAD** (main), **feature-update**. Khi cùng một dòng bị sửa ở cả hai nhánh → conflict → phải chọn/gộp tay rồi commit merge.
