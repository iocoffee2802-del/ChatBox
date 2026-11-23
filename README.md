# Chatbot theo tài liệu – OpenAI + Admin-only Knowledge Base

## Quyền truy cập
- Khách truy cập: chỉ được hỏi đáp.
- Admin: đăng nhập bằng `ADMIN_PASSWORD`, được nạp và xóa tài liệu.
- Tài liệu được lưu tập trung trên máy chủ; mọi khách truy cập sử dụng chung kho kiến thức.
- API Key OpenAI chỉ tồn tại trong biến môi trường của máy chủ.

## Chạy thử trên máy tính
Yêu cầu Node.js 20 trở lên.

1. Cài thư viện: `npm install`
2. Sao chép `.env.example` thành `.env`.
3. Điền `OPENAI_API_KEY` và đặt mật khẩu mạnh cho `ADMIN_PASSWORD`.
4. Chạy `npm start`.
5. Mở `http://localhost:3000`.

## Deploy GitHub + Render
1. Tạo Web Service từ repository này.
2. Build command: `npm install`; Start command: `npm start`.
3. Thêm Environment Variables `OPENAI_API_KEY`, `OPENAI_MODEL`, `ADMIN_PASSWORD`, `NODE_ENV=production`, `DATA_DIR=/var/data`.
4. Gắn Persistent Disk vào `/var/data`.

## Lưu trữ
Kho tài liệu ghi tại `DATA_DIR/documents.json`. Cần persistent disk để dữ liệu không mất khi deploy/restart.

## Bảo mật và giới hạn
- API Key chỉ lưu ở máy chủ; không đưa `.env` lên GitHub.
- Cookie Admin HttpOnly, SameSite=Strict; Secure khi production.
- Phiên Admin hết hạn sau 8 giờ; restart sẽ yêu cầu đăng nhập lại.
- Giới hạn đăng nhập 10 lần/15 phút; hỏi đáp 20 lần/phút/IP.
- Một mật khẩu Admin chung; tài liệu PDF scan không có text layer chưa được OCR.
- Upload được trích xuất thành text ở trình duyệt; server không lưu PDF gốc.
