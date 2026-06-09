# Tiêu chuẩn Chuẩn hóa API Response

## 1. Mô tả Quy tắc
Quy tắc này bắt buộc tất cả các API trả về từ Controller đều phải đi qua `TransformInterceptor` (hoặc cấu trúc tương đương được định nghĩa trong `libs/core`) để đồng nhất format dữ liệu trả về cho Frontend, thường là `{ statusCode, message, data }`.

## 2. Quy tắc thực thi (BẮT BUỘC)
- **Tuyệt đối KHÔNG** tự bọc dữ liệu bằng tay trong Controller (ví dụ: `return { data: result, statusCode: 200 }`). 
- Controller chỉ được phép trả về kiểu dữ liệu gốc nguyên bản (ví dụ: `return user` hoặc `return usersArray`). Hệ thống Interceptor toàn cục sẽ tự động bắt lấy và bọc lại vào form chuẩn.
- Đối với trường hợp báo lỗi (Exception), bắt buộc phải sử dụng các class Exception của NestJS (như `BadRequestException`, `NotFoundException`) để ném ra, tuyệt đối không trả về object có chứa cờ báo lỗi bằng tay. Hệ thống `Global Exception Filter` sẽ tự bắt và format lỗi đồng nhất.
