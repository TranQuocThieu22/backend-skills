---
name: git-commit-convention
description: Quy định bắt buộc khi viết commit message cho dự án backend
---

# Quy Định Viết Git Commit Message (Backend)

Khi thực hiện tạo commit cho các dự án backend, bạn **bắt buộc** phải tuân thủ các quy tắc sau:

1. **Ngôn ngữ**: Commit message phải được viết bằng **Tiếng Việt**.
2. **Thành phần/API bị ảnh hưởng**: Bắt buộc phải lưu/ghi chú rõ **tên API, Service hoặc chức năng bị ảnh hưởng** bởi thay đổi trong nội dung commit (ví dụ: `=> Thành phần bị ảnh hưởng: API lấy danh sách hoạt động cộng đồng / Service gửi email`).
3. **Cấu trúc**:
   - Tiêu đề commit (dòng 1): Theo chuẩn `<type>(<scope>): <short summary>`.
     - `<type>` bắt buộc phải là một trong các loại chuẩn: `feat`, `fix`, `chore`, `ci`, `build`, `docs`, `refactor`, `test`, `perf`, `style`. Tuyệt đối KHÔNG DÙNG từ không chuẩn như `update` hoặc "Cập nhật".
     - `<scope>` (trong ngoặc đơn) **BẮT BUỘC** là tên service/module/package bị ảnh hưởng (ví dụ: `sae-api`, `aq-core-framework`, `auth-service`). Nếu thay đổi từ 2 module trở lên, phân tách bằng dấu phẩy (ví dụ: `refactor(sae-api,auth-service): ...`). Tuyệt đối không dùng tên folder tính năng nhỏ làm scope.
     - `<short summary>` **BẮT BUỘC** phải viết bằng **Tiếng Việt** (ví dụ: `tối ưu hoá query database và xoá file utils cũ`).
   - Body commit: Giải thích chi tiết hơn về thay đổi (nếu cần thiết) và dòng ghi chú thành phần/API bị ảnh hưởng.
4. **Quy trình thực hiện**: Tuyệt đối **KHÔNG** tự ý thực thi các lệnh `git commit`. Lệnh cấm này áp dụng cho mọi công cụ, đặc biệt là công cụ `run_command`. BẠN KHÔNG BAO GIỜ ĐƯỢC CHẠY `git commit` TRONG TERMINAL. Bạn chỉ được phép sinh ra nội dung commit message và in ra trong phần chat để người dùng tự xem xét (review) và tự commit.
5. **Định dạng hiển thị**: TUYỆT ĐỐI KHÔNG sinh ra tin nhắn commit dưới dạng lệnh Terminal (ví dụ `git commit -m "..."`). Hãy in TOÀN BỘ nội dung commit (bao gồm Tiêu đề, một dòng trống, và Body) gộp chung vào **MỘT khối code block duy nhất** dạng text thô. Không tách rời tiêu đề và body ra văn bản riêng, để người dùng có thể bấm Copy một lần và dán (paste) thẳng vào giao diện Git GUI (như Fork, SourceTree) cho nhanh.
