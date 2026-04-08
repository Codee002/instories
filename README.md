# Instories

Instories là dự án mạng xã hội full-stack được xây dựng theo mô hình tách riêng `front_end` (Vuejs) và `back_end` (Laravel). Ứng dụng hướng tới trải nghiệm tương tác thời gian thực: người dùng có thể đăng bài, tạo story, kết bạn, nhắn tin, nhận thông báo tức thì và thực hiện gọi thoại/video ngay trong cuộc trò chuyện.

Repo này phù hợp để đọc theo góc nhìn một sản phẩm hoàn chỉnh: có luồng xác thực, khu vực người dùng, khu vực quản trị, broadcast realtime và tích hợp dịch vụ bên thứ ba.

## 1. Giới thiệu dự án

**Tên đề tài:** Xây dựng hệ thống mạng xã hội    
Mục tiêu của Instories là xây dựng một nền tảng mạng xã hội mini với các nhóm tính năng chính:

- Quản lý tài khoản và hồ sơ cá nhân
- Tạo và tương tác với bài viết
- Story ngắn hạn
- Kết bạn và quản lý mối quan hệ
- Nhắn tin realtime
- Gọi video qua Agora
- Thông báo realtime
- Quản trị nội dung và tài khoản

## 2. Chức năng chính

### Người dùng

- Đăng ký, đăng nhập, đăng xuất bằng Laravel Sanctum
- Quên mật khẩu qua email
- Xác thực email và bật/tắt xác thực 2 bước
- Cập nhật hồ sơ cá nhân: tên, email, số điện thoại, ngày sinh, giới tính, địa chỉ, bio
- Cập nhật avatar và ảnh bìa
- Tìm kiếm người dùng
- Kết bạn, nhận lời mời kết bạn, thay đổi trạng thái quan hệ
- Xem danh sách bạn bè và lời mời kết bạn

### Bài viết và tương tác

- Tạo bài viết kèm ảnh hoặc video
- Hiển thị bảng tin theo mức độ tương tác
- Xem chi tiết bài viết
- Thích, bình luận, chia sẻ, xem bài viết
- Lưu danh sách bài viết đã thích
- Xem các bài viết đã chia sẻ
- Báo cáo bài viết
- Xóa bài viết, xóa bình luận

### Story

- Tạo story ảnh/video
- Hiển thị story từ bạn bè và chính người dùng
- Theo dõi lượt xem story
- Xem chi tiết story

### Nhắn tin và gọi

- Tạo cuộc trò chuyện 1-1
- Tạo conversation nhóm
- Gửi tin nhắn văn bản
- Gửi tin nhắn kèm media
- Thu hồi tin nhắn
- Chia sẻ bài viết qua khung chat
- Nhận tin nhắn realtime bằng broadcast event
- Bắt đầu cuộc gọi từ cửa sổ chat
- Chấp nhận cuộc gọi và join cùng channel Agora

### Thông báo và quản trị

- Nhận thông báo realtime khi có tương tác mới
- Đánh dấu thông báo đã đọc
- Báo cáo người dùng
- Trang quản trị tài khoản
- Trang quản trị bài viết
- Duyệt danh sách báo cáo tài khoản và báo cáo bài viết
- Khóa/mở tài khoản hoặc thay đổi trạng thái bài viết

## 3. Công nghệ sử dụng

### Frontend

- Vue 3
- Vue Router 4
- Ant Design Vue
- Axios
- Vue Toastification
- Day.js
- Laravel Echo
- Pusher JS
- Agora RTC SDK

### Backend

- Laravel 12
- PHP 8.2+
- Laravel Sanctum
- Eloquent ORM
- Laravel Broadcasting
- Laravel Mail
- Pest

### Dữ liệu và lưu trữ

- SQLite mặc định theo cấu hình Laravel 12, có thể đổi sang MySQL/MariaDB
- Upload media bằng Laravel Storage (`storage/app/public`)

## 4. Các dịch vụ và tích hợp nổi bật

### 1. Sanctum

Xử lý xác thực API bằng token cho frontend Vue.

### 2. Pusher + Laravel Echo

Được dùng cho các tính năng realtime như:

- tin nhắn mới
- lời mời kết bạn
- thông báo hệ thống
- cập nhật story/bài viết
- tín hiệu bắt đầu cuộc gọi

### 3. Agora

Dùng để tạo token RTC và thực hiện cuộc gọi trong cửa sổ chat.

### 4. Mail

Dùng cho:

- gửi link quên mật khẩu
- gửi link xác thực email
- gửi link xác thực 2 bước khi đăng nhập

## 5. Cấu trúc thư mục

```text
instories/
├── back_end/      # Laravel API, auth, database, broadcast, mail, Agora token
├── front_end/     # Vue 3 client cho user/admin
├── README.md
└── B2205955_TranThanhPhuc.pdf
```

### `back_end`

- `app/Http/Controllers/Api/AuthController.php`: xác thực, quên mật khẩu, email, 2FA
- `app/Http/Controllers/Api/User/*`: người dùng, bài viết, story, quan hệ, chat
- `app/Http/Controllers/Api/Admin/*`: quản trị tài khoản và bài viết
- `app/Events/*`: các event broadcast realtime
- `routes/api.php`: định nghĩa API cho auth, user, admin
- `routes/channels.php`: private channel cho broadcast
- `database/migrations/*`: schema cho users, profiles, posts, stories, conversations, reports...

### `front_end`

- `src/router/*`: route cho auth, user, admin
- `src/pages/auth/*`: giao diện đăng nhập, đăng ký, reset mật khẩu
- `src/pages/user/*`: dashboard, profile, friend, conversation, story, settings
- `src/pages/admin/*`: màn hình quản trị
- `src/main.js`: cấu hình Axios, Pusher/Echo, theme, router

## 6. Luồng nghiệp vụ chính

### Luồng người dùng

1. Người dùng đăng ký tài khoản
2. Hệ thống tạo `user` và `profile`
3. Người dùng đăng nhập để nhận `auth_token`
4. Frontend dùng token để gọi API và subscribe channel realtime
5. Người dùng có thể đăng bài, tạo story, nhắn tin, kết bạn, nhận thông báo

### Luồng realtime

1. Backend phát sự kiện thông qua Laravel Broadcasting
2. Pusher chuyển sự kiện tới private channel của từng user
3. Frontend lắng nghe bằng Laravel Echo
4. Giao diện cập nhật ngay mà không cần reload trang

### Luồng gọi video

1. User A tạo cuộc gọi từ conversation
2. Backend tạo `channel` và `token` Agora
3. Event call được broadcast đến người nhận
4. User B chấp nhận cuộc gọi
5. Cả hai cùng join vào một Agora channel

## 7. Hướng dẫn chạy dự án local

## Backend

```bash
cd back_end
composer install
php artisan key:generate
php artisan migrate
php artisan storage:link
php artisan serve
```

Nếu dùng SQLite, cần tạo file `database/database.sqlite` trước khi migrate nếu máy chưa có sẵn.

## Frontend

```bash
cd front_end
npm install
npm run serve
```

## 8. Biến môi trường cần chú ý

### Backend `.env`

Những biến quan trọng đang được code sử dụng trực tiếp:

```env
APP_NAME=Instories
APP_URL=http://127.0.0.1:8000
CLIENT_URL=http://localhost:8080/

DB_CONNECTION=sqlite
# hoặc mysql / mariadb

MAIL_MAILER=smtp
MAIL_HOST=...
MAIL_PORT=...
MAIL_USERNAME=...
MAIL_PASSWORD=...
MAIL_FROM_ADDRESS=...

BROADCAST_DRIVER=pusher
PUSHER_APP_ID=...
PUSHER_APP_KEY=...
PUSHER_APP_SECRET=...
PUSHER_APP_CLUSTER=ap1
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https

AGORA_APP_ID=...
AGORA_CERTIFICATE=...
```

### Frontend `.env`

```env
VUE_APP_DOMAIN_API=http://127.0.0.1:8000/api
VUE_APP_BACKEND_URL=http://127.0.0.1:8000

VUE_APP_PUSHER_APP_KEY=...
VUE_APP_PUSHER_APP_SCHEME=https

VUE_APP_AGORA_APP_ID=...
```

## 9. Một số endpoint tiêu biểu

### Auth

- `POST /api/auth/register`
- `POST /api/auth/login`
- `POST /api/auth/forgot`
- `POST /api/auth/handleReset`
- `POST /api/auth/activeEmail`

### User

- `GET /api/owner`
- `GET /api/getPosts/{id}`
- `GET /api/story/getDashboardStories`
- `POST /api/post/store`
- `POST /api/conversation/sendMessage`
- `POST /api/conversation/startCall`

### Admin

- `GET /api/admin/account/getAccounts`
- `POST /api/admin/account/changeAccountStatus`
- `GET /api/admin/post/getPosts`
- `POST /api/admin/post/changePostStatus`

## 10. Tài liệu bổ sung trong repo

- `back_end/AGORA_CALL_EXPLANATION.md`: giải thích chi tiết flow call và token Agora
- `back_end/PUSHER_GUIDE.md`: giải thích cấu hình Pusher, private channel và event broadcast
- `B2205955_TranThanhPhuc.pdf`: báo cáo đồ án

## 11. Điểm nổi bật của dự án

- Có đầy đủ cả frontend, backend và nghiệp vụ quản trị
- Có broadcast realtime thay vì chỉ CRUD cơ bản
- Có tích hợp Agora cho call, giúp dự án nổi bật hơn bài tập web thông thường
- Có luồng email thực tế: xác thực email, quên mật khẩu, xác thực 2 bước
- Mô hình dữ liệu khá đầy đủ cho một mạng xã hội mini: user, profile, relation, post, comment, share, like, story, conversation, message, notification, report

## 12. Ghi chú

- Repo hiện có chỉnh sửa chưa commit ở một số file nguồn; README này chỉ mô tả theo trạng thái code hiện tại.
- File `back_end/.env.example` hiện không có trong repo, nên khi setup local cần tự tạo `.env` từ mẫu Laravel tương ứng rồi bổ sung các biến ở trên.
