---
name: multi-tenant-architecture
description: Quy định kiến trúc và best practices khi code dự án Multi-tenant NestJS (Database động theo từng trường)
---

# Multi-Tenant Architecture Rules

Khi tham gia phát triển hoặc chỉnh sửa các tính năng liên quan đến dữ liệu người dùng (dữ liệu thuộc về tenant/trường học), bạn **BẮT BUỘC** phải tuân theo các quy tắc kiến trúc sau đây để đảm bảo hệ thống Multi-tenant hoạt động ổn định và không gặp lỗi Runtime.

## 1. KHÔNG SỬ DỤNG `TypeOrmModule.forFeature()`
Trong kiến trúc Multi-tenant hiện tại, Database connection được tạo động vào lúc Runtime (dựa trên Request của người dùng). Bạn không được sử dụng `TypeOrmModule.forFeature([Entity])` tĩnh bên trong các Module.
- ❌ **Sai:** `@Module({ imports: [TypeOrmModule.forFeature([AccountEntity])] })`
- Việc sử dụng `.forFeature()` tĩnh sẽ khiến NestJS cố gắng kết nối DB Master để tạo Repository lúc app khởi động và gây sập hệ thống hoặc phình to bộ nhớ.

## 2. Đăng ký Entities bằng `TenantEntityRegistry`
Vì dự án build bằng Webpack, các hàm quét đường dẫn tự động (như Glob string `**/*.entity.ts`) sẽ gây lỗi `SyntaxError`.
- ✅ **Đúng:** Bạn phải đăng ký tường minh các Entity của mình trực tiếp vào `TenantEntityRegistry` ở đầu file khai báo Module.
```typescript
import { TenantEntityRegistry } from '@app/core/tenant/tenant-entity.registry';
import { AccountEntity } from './entities/account.entity';

TenantEntityRegistry.register([AccountEntity]);

@Module({
  // ...
})
export class AccountModule {}
```

## 3. Khởi tạo Repository Động Trong Service
Thay vì inject tĩnh Repository thông qua Decorator `@InjectRepository()`, hãy sử dụng `TenantConnectionService` để lấy Repository động tuỳ theo context của request hiện tại.
- ✅ **Cách tiếp cận:**
```typescript
import { TenantConnectionService } from '@app/core/tenant/tenant-connection.service';

@Injectable()
export class AccountService {
  constructor(private readonly tenantConnection: TenantConnectionService) {}

  async findAll() {
    const repo = await this.tenantConnection.getRepository(AccountEntity);
    return repo.find();
  }
}
```

## 4. Kế thừa `AbstractTenantEntity`
Mọi Entity quản lý dữ liệu của người dùng/hệ thống Tenant đều phải được kế thừa từ `AbstractTenantEntity` (chứa sẵn ID, các trường Audit chuẩn như `createdAt`, `updatedAt`, `deletedAt`, và `tenantId`).
- Hạn chế tự định nghĩa lại các Primary Key trừ khi đó là bảng đặc thù.

## 5. Middleware & Context (CLS)
Mã `tenantId` đã được trích xuất tự động qua `TenantMiddleware` (Ưu tiên: 1. Header `x-tenant-id`, 2. Token JWT Keycloak Issuer).
- Mã này được lưu trữ an toàn xuyên suốt Request thông qua `ClsService` (của `nestjs-cls`).
- 💡 **Mẹo:** Bạn không cần bắt buộc người dùng truyền `tenantId` vào body request. Thay vào đó, hãy lấy từ `ClsService` hoặc hệ thống sẽ tự động liên kết tới DB dựa trên `ClsService`.

## 6. Lưu ý thêm về thứ tự Middleware
Cấu hình Global phải đảm bảo `ClsMiddleware` luôn được gọi **trước** `TenantMiddleware` để context được khởi tạo thành công trước khi gán `tenantId`.
