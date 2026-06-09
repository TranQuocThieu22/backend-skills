# Tiêu chuẩn Kiến trúc Thư mục NestJS

## 1. Mô tả Quy tắc
Quy tắc này bắt buộc AI phải tuân thủ nghiêm ngặt mô hình kiến trúc thư mục phân lớp (Modular Architecture) khi phát triển bất kỳ tính năng, ứng dụng (app) hay thư viện (libs) nào trong Monorepo NestJS này. Mục tiêu là đảm bảo mã nguồn dễ bảo trì, dễ mở rộng và có tính đồng nhất tuyệt đối giữa các module.

## 2. Cấu trúc Bắt buộc
Khi khởi tạo một Library mới trong `libs/` hoặc một Module mới trong `apps/`, CẤM được để phẳng (flat) các file ở thư mục gốc (ngoại trừ file `.module.ts` và `index.ts`). Thay vào đó, phải chia vào đúng các thư mục chuyên trách sau:

- `controllers/`: Chứa các file Controller (`*.controller.ts`). Chỉ nhận HTTP Request, gọi sang Service và trả về Response. Tuyệt đối không chứa logic nghiệp vụ.
- `dto/`: Chứa các file Data Transfer Object (`*.dto.ts`) để validate dữ liệu đầu vào.
- `entities/`: Chứa các file định nghĩa CSDL TypeORM (`*.entity.ts`).
- `repositories/`: Chứa các file Custom Repository (`*.repository.ts`) để thực hiện các câu truy vấn phức tạp thay vì gọi trực tiếp từ Service.
- `services/`: Chứa các file logic nghiệp vụ (`*.service.ts`). Tầng này thực hiện xử lý cốt lõi.
- `guards/` (tuỳ chọn): Chứa các file Guard (`*.guard.ts`) bảo vệ API (phân quyền, kiểm tra token).
- `strategies/` (tuỳ chọn): Chứa các file Passport Strategy (như JWT, OAuth2).
- `interceptors/` (tuỳ chọn): Chứa các Interceptor định dạng lại dữ liệu.

## 3. Quy trình thực hiện
Mỗi khi khởi tạo một Library mới, AI phải chủ động dùng lệnh `mkdir` để tạo sẵn toàn bộ các thư mục con này (như `controllers`, `services`, `dto`, `entities`, `repositories`) kèm theo một file `.gitkeep` bên trong mỗi thư mục nếu chưa có code, để đảm bảo cấu trúc được hiển thị rõ ràng trên IDE của người dùng. Tuyệt đối không được quên!
