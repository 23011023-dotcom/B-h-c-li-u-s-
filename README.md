# Toán 3 – Kho học liệu số

**Slogan:** Học liệu sáng tạo – Kết nối tri thức – Lan tỏa niềm vui học Toán.

Website cộng đồng hỗ trợ giáo viên tiểu học tìm kiếm, khai thác và chia sẻ học liệu số phục vụ dạy học mạch kiến thức **Số và phép tính – Toán lớp 3**.

## 1. Công nghệ

- Next.js 15 (App Router), React 19, TypeScript
- Tailwind CSS
- Prisma ORM + SQLite (phù hợp chạy thử và phát triển local)
- NextAuth.js (đăng nhập bằng email/mật khẩu)
- bcryptjs (băm mật khẩu), Lucide React

## 2. Chức năng trong bản hiện tại

- Trang chủ, thống kê, học liệu nổi bật và chủ đề.
- Kho học liệu, tìm kiếm và lọc theo tiêu chí có sẵn.
- Trang chi tiết học liệu.
- Đăng ký, đăng nhập và phân quyền thành viên/quản trị viên.
- Thành viên gửi học liệu bằng đường dẫn; học liệu mới ở trạng thái chờ duyệt.
- Quản trị viên duyệt hoặc từ chối học liệu.
- Danh sách chủ đề và trang hướng dẫn.

**Phạm vi cần lưu ý:** Bản hiện tại là MVP để chạy thử và phát triển tiếp. Form chia sẻ nhận đường dẫn học liệu, chưa triển khai tải tệp trực tiếp lên máy chủ. Một số bảng dữ liệu như yêu thích, bình luận, đánh giá và báo cáo đã có trong Prisma schema nhưng giao diện/API tương ứng chưa hoàn thiện. Dữ liệu seed là dữ liệu minh họa, không phải học liệu thật; cần thay bằng tài nguyên có quyền sử dụng trước khi giới thiệu rộng rãi.

## 3. Yêu cầu

- Node.js 20 trở lên (khuyến nghị bản LTS)
- npm

Kiểm tra phiên bản:

```bash
node -v
npm -v
```

## 4. Cài đặt và chạy trên máy

Mở Terminal tại thư mục `toan3-kho-hoc-lieu`:

```bash
# 1. Cài thư viện
npm install

# 2. Tạo file cấu hình môi trường
# Windows PowerShell:
Copy-Item .env.example .env
# macOS/Linux:
# cp .env.example .env

# 3. Tạo Prisma Client
npm run db:generate

# 4. Tạo database và dữ liệu minh họa
npm run db:push
npm run db:seed

# 5. Chạy website
npm run dev
```

Mở http://localhost:3000.

Nếu dùng PowerShell, chỉ chạy dòng `Copy-Item`; nếu dùng macOS/Linux, chạy dòng `cp` tương ứng.

## 5. Cấu hình `.env`

File `.env.example` là mẫu. Sao chép thành `.env` rồi đặt `NEXTAUTH_SECRET` thành chuỗi bí mật ngẫu nhiên của riêng bạn. Không tải `.env` lên GitHub.

Ví dụ tạo secret bằng Node.js:

```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

Dán kết quả vào `NEXTAUTH_SECRET` trong `.env`.

## 6. Tài khoản minh họa

Sau khi chạy `npm run db:seed`:

| Vai trò | Email | Mật khẩu |
|---|---|---|
| Quản trị viên | admin@toan3.edu.vn | admin123 |
| Giáo viên | gv.nguyenthia@toan3.edu.vn | teacher123 |
| Giáo viên | gv.tranvanb@toan3.edu.vn | teacher123 |

**Chỉ dùng các tài khoản này để thử nghiệm local.** Không dùng mật khẩu mẫu trên website công khai. Trước khi triển khai thật, cần thay đổi hoặc loại bỏ tài khoản mẫu, tạo tài khoản quản trị riêng và cấu hình secret riêng.

## 7. Các lệnh thường dùng

```bash
npm run dev          # Chạy môi trường phát triển
npm run build        # Build production
npm run start        # Chạy bản production sau khi build
npm run lint         # Kiểm tra ESLint
npm run db:generate  # Sinh Prisma Client
npm run db:push      # Đồng bộ schema với database
npm run db:seed      # Tạo dữ liệu minh họa
npm run db:studio    # Mở Prisma Studio
npm run db:reset     # Xóa database và tạo lại dữ liệu mẫu (CẨN THẬN: mất dữ liệu cũ)
```

## 8. Đưa mã nguồn lên GitHub

Tạo repository mới trên GitHub, ví dụ `toan3-kho-hoc-lieu`. Không chọn tạo README/.gitignore nếu thư mục dự án đã có các file này. Sau đó chạy trong Terminal tại thư mục dự án:

```bash
git init
git add .
git status
git commit -m "feat: initial Toan 3 digital learning resources app"
git branch -M main
git remote add origin https://github.com/<TEN_TAI_KHOAN_GITHUB>/toan3-kho-hoc-lieu.git
git push -u origin main
```

Thay `<TEN_TAI_KHOAN_GITHUB>` bằng tên tài khoản GitHub của bạn. Trước `git commit`, kiểm tra `git status` để chắc chắn `.env`, database và thư mục `node_modules` không xuất hiện trong danh sách được commit.

## 9. Lưu ý khi triển khai website công khai

SQLite trong cấu hình hiện tại phù hợp chạy local/demo. Không nên triển khai bản production có nhiều người dùng trên nền tảng serverless với SQLite cục bộ vì dữ liệu có thể không bền vững giữa các lần chạy. Khi deploy, cần chuyển Prisma datasource sang PostgreSQL, cấu hình `DATABASE_URL` và `NEXTAUTH_SECRET` trong phần Environment Variables của nền tảng triển khai, sau đó chạy migration/schema phù hợp. Tính năng lưu file cũng cần dùng dịch vụ lưu trữ bền vững thay vì thư mục local.

## 10. Cấu trúc thư mục

```text
toan3-kho-hoc-lieu/
├── prisma/
│   ├── schema.prisma
│   └── seed.ts
├── public/
│   └── uploads/
├── src/
│   ├── app/
│   │   ├── api/                 # API đăng ký, xác thực, học liệu
│   │   ├── admin/               # Quản trị và duyệt học liệu
│   │   ├── chia-se/             # Gửi học liệu
│   │   ├── chu-de/              # Danh sách chủ đề
│   │   ├── dang-ky/             # Đăng ký
│   │   ├── dang-nhap/           # Đăng nhập
│   │   ├── hoc-lieu/[slug]/     # Chi tiết học liệu
│   │   ├── huong-dan/           # Hướng dẫn
│   │   └── kho-hoc-lieu/        # Kho học liệu
│   ├── components/
│   ├── lib/
│   └── types/
├── .env.example
├── .gitignore
├── package.json
└── README.md
```

---

Phát triển với tình yêu dành cho giáo dục tiểu học Việt Nam.
