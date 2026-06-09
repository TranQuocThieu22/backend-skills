# Tiêu chuẩn Cấu hình Biến Môi Trường (Env)

## 1. Mô tả Quy tắc
Đảm bảo hệ thống sẽ Crash (chết) ngay từ lúc khởi động (Fail-fast) nếu DevOps hoặc người cài đặt On-Premise quên thiết lập các biến môi trường thiết yếu (như DB Password, JWT Secret). Nghiêm cấm gọi biến môi trường tuỳ tiện ở giữa logic nghiệp vụ.

## 2. Quy tắc thực thi (BẮT BUỘC)
- **CẤM** gọi trực tiếp `process.env.VARIABLE_NAME` trong bất kỳ file Services, Controllers hay Repositories nào.
- Sử dụng module `@nestjs/config`.
- Mọi biến môi trường phải được định nghĩa Schema (Joi) hoặc Class Validator tại module Config tập trung (thường ở `libs/core` hoặc module Config chuyên biệt) để kiểm tra bắt buộc tính hợp lệ (ví dụ: DB_PORT bắt buộc là số).
- Bất kỳ chỗ nào cần dùng đến cấu hình, phải Inject `ConfigService` và lấy dữ liệu qua service này. Ví dụ: `this.configService.get<string>('DATABASE_HOST')`.
