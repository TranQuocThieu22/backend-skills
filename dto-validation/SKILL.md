# Tiêu chuẩn Validate Dữ liệu (DTO)

## 1. Mô tả Quy tắc
Mọi API tiếp nhận dữ liệu từ Frontend (Body, Query, Params) đều phải có cơ chế xác thực đầu vào khắt khe nhằm chống lại lỗ hổng bảo mật rác dữ liệu (Mass Assignment) và để hệ thống tự động sinh tài liệu Swagger chuẩn xác.

## 2. Quy tắc thực thi (BẮT BUỘC)
- **Mọi dữ liệu đầu vào** đều phải được định nghĩa bằng các file `*.dto.ts`. Tuyệt đối không dùng kiểu `any` hoặc lấy raw body qua `@Body() body: any`.
- Phải gắn đầy đủ các Decorator của `class-validator` (như `@IsString()`, `@IsOptional()`, `@IsNotEmpty()`, `@IsEmail()`) cho TẤT CẢ các properties trong DTO.
- Phải gắn Decorator của `@nestjs/swagger` (như `@ApiProperty()`, `@ApiPropertyOptional()`) để Swagger bóc tách mô tả giao diện API.
- Ở tầng khởi tạo cấu hình `ValidationPipe` toàn cục (trong `main.ts`), LUÔN LUÔN phải kích hoạt cờ `whitelist: true` (chỉ chấp nhận các trường có định nghĩa trong DTO) và `forbidNonWhitelisted: true` (ném lỗi HTTP 400 nếu Frontend gửi thừa một trường dữ liệu không nằm trong DTO).
