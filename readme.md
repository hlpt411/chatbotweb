# PTBot — Trợ lý AI tiếng Việt 🤖

> Một giao diện chat AI hoàn chỉnh trong **một file HTML duy nhất** — không cần build, không cần framework, mở là chạy.
> Được tạo bởi **PT** 🇻🇳

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)
![No Build](https://img.shields.io/badge/Zero%20Dependencies-✓-brightgreen?style=flat-square)

---

## 📖 Giới thiệu

PTBot là giao diện web chat AI với giao diện tối (dark mode) hiện đại, tối ưu cho tiếng Việt. Nó kết nối tới bất kỳ API tương thích chuẩn **OpenAI Chat Completions** (OpenAI, DeepSeek, OpenRouter, Groq, các dịch vụ self-host như Ollama + LiteLLM, v.v.) và trả lời như một trợ lý AI mang danh tính riêng: **PTBot**.

Toàn bộ ứng dụng chỉ là 1 file — mở trực tiếp bằng trình duyệt hoặc host lên bất kỳ static hosting nào là dùng được.

---

## ✨ Tính năng chính

| Nhóm | Chi tiết |
|------|----------|
| 💬 Chat | Gửi/nhận tin nhắn với API Chat Completions, hiệu ứng "đang nhập..." (typing indicator) |
| 🗂️ Lịch sử | Nhiều cuộc trò chuyện, lưu tự động vào `localStorage`, đổi tên theo tin nhắn đầu, xóa từng cuộc / xóa tất cả |
| 📝 Markdown | Render **in đậm**, *in nghiêng*, tiêu đề, danh sách, trích dẫn, `code inline` |
| 💻 Code block | Tự tách khối code, có nút **Sao chép**, cuộn ngang khi code dài |
| 🔄 Tiện ích | Sao chép tin nhắn, **Tạo lại** (regenerate) câu trả lời, **Thử lại** khi lỗi |
| 🧠 Danh tính | System prompt riêng (persona) + bộ lọc `sanitizeResponse()` tự xóa nội dung lệch danh tính |
| 📱 Responsive | Sidebar trượt trên mobile, giao diện co giãn mọi kích thước màn hình |
| ⚠️ Xử lý lỗi | Phân loại lỗi rõ ràng: mạng, API key sai (401), rate limit (429), server lỗi (5xx), kèm toast thông báo |
| ⌨️ Phím tắt | `Enter` gửi, `Shift + Enter` xuống dòng, `/` focus ô nhập |

---

## 🛠️ Công nghệ

- **HTML + CSS + JavaScript thuần** — không framework, không thư viện JS
- Font: [Google Fonts](https://fonts.google.com/) (Inter + JetBrains Mono) — có sẵn fallback nếu mất mạng
- Dữ liệu lưu trong `localStorage` của trình duyệt (key: `ptbot_conversations`)
- Gọi API qua `fetch()` tới endpoint `/chat/completions` (chuẩn OpenAI-compatible)

---

## 🚀 Cài đặt & cấu hình

### 1. Đổi tên file

File hiện đang có đuôi `.txt`. Đổi thành file HTML:

```
chatbot.html.txt  →  chatbot.html
```

### 2. Cấu hình API

Mở file bằng trình soạn thảo (VS Code, Notepad++...), tìm khối `API_CONFIG` ở phần đầu `<script>`:

```js
const API_CONFIG = {
  baseUrl: 'Your base url',
  apiKey: 'Your api key',
  model: 'Your model'
};
```

Thay bằng thông tin thật của bạn:

| Trường | Mô tả | Ví dụ |
|--------|-------|-------|
| `baseUrl` | Base URL của API, **không kèm** `/chat/completions` | `https://api.openai.com/v1` · `https://api.deepseek.com/v1` · `https://openrouter.ai/api/v1` |
| `apiKey` | API key của bạn | `sk-...` |
| `model` | Tên model muốn dùng | `gpt-4o-mini` · `deepseek-chat` · `llama-3.1-70b` |

> 💡 **Lưu ý:** file tự nối thêm `/chat/completions` vào `baseUrl`, vì vậy mọi dịch vụ theo chuẩn OpenAI đều dùng được.

### 3. Chạy

**Cách 1 — Mở trực tiếp:** double-click file `chatbot.html`, trình duyệt sẽ mở.

**Cách 2 — Host lên mạng:** đẩy lên GitHub Pages, Netlify, Vercel, Cloudflare Pages... để dùng trên điện thoại hoặc chia sẻ cho người khác.

---

## ⌨️ Phím tắt

| Phím | Chức năng |
|------|-----------|
| `Enter` | Gửi tin nhắn |
| `Shift + Enter` | Xuống dòng |
| `/` | Nhảy vào ô nhập (khi đang không gõ chữ) |

---

## 🧱 Cấu trúc code (trong 1 file)

```
chatbot.html
├── <head> — Meta, Google Fonts, toàn bộ CSS (dark theme, responsive, animation)
└── <body>
    ├── Sidebar — Logo, nút chat mới, danh sách lịch sử, badge tác giả
    ├── Chat header — Tên model, nút xóa tất cả
    ├── Messages container — Màn hình chào + các tin nhắn
    ├── Input area — Ô nhập đa dòng, nút gửi, gợi ý phím tắt
    └── <script>
        ├── API_CONFIG            — Nơi bạn điền API key (QUAN TRỌNG)
        ├── PTBOT_PERSONA         — System prompt định danh PTBot
        ├── sanitizeResponse()    — Bộ lọc nội dung phản hồi
        ├── callAPI()             — Gọi API Chat Completions
        ├── renderChat / renderHistory — Vẽ giao diện
        ├── formatInlineMarkdown / parseMessageParts — Render Markdown + code block
        └── Event handlers       — Submit form, phím Enter, phím tắt
```

---

## 🔒 Lưu ý bảo mật (quan trọng!)

⚠️ **API key nằm thẳng trong file HTML = bất kỳ ai xem mã nguồn (F12) đều lấy được.**

- ✅ **An toàn:** dùng cho cá nhân, localhost, hoặc nhóm nhỏ tin cậy.
- ❌ **Không an toàn:** public lên mạng cho người lạ dùng chung với key thật của bạn.
- 💡 **Giải pháp khi muốn public:** dựng một backend proxy nhỏ (ví dụ: Cloudflare Worker, Vercel Serverless Function) giữ key ở server, frontend gọi qua proxy. Hoặc dùng các dịch vụ có key rẻ/giới hạn chi tiêu (OpenRouter, Groq...) để giảm rủi ro.

---

## 🩺 Xử lý sự cố thường gặp

| Hiện tượng | Nguyên nhân | Cách khắc phục |
|-----------|-------------|----------------|
| `API key không hợp lệ` (401) | Key sai hoặc hết hạn | Kiểm tra lại `apiKey` |
| `Lỗi mạng — không kết nối được server` | Sai `baseUrl`, mất mạng, hoặc CORS | Kiểm tra URL; một số dịch vụ chặn gọi từ trình duyệt (CORS) — cần dùng proxy backend |
| `Quá nhiều yêu cầu` (429) | Vượt rate limit | Chờ một lát rồi thử lại |
| `Server đang gặp sự cố` (5xx) | Lỗi phía nhà cung cấp | Thử lại sau |
| Bot trả lời trống | Model không hỗ trợ hoặc prompt lỗi | Kiểm tra tên `model`, đổi model khác thử |
| Mất hết lịch sử chat | Bị xóa `localStorage` | Dữ liệu chỉ nằm ở trình duyệt — xóa dữ liệu trình duyệt là mất chat |

---

## 🧩 Tùy chỉnh

- **Đổi tính cách bot:** sửa biến `PTBOT_PERSONA` và `PTBOT_REINFORCE` trong `<script>`.
- **Đổi màu giao diện:** sửa các biến CSS trong `:root` (đầu thẻ `<style>`), ví dụ `--accent-primary`, `--bg-primary`.
- **Đổi tên bot:** tìm thay thế chữ `PTBot` (nhớ sửa cả `sanitizeResponse()` cho khớp).
- **Chỉnh thông số sinh văn bản:** sửa `temperature`, `max_tokens` trong hàm `callAPI()`.

---

## 👤 Tác giả

**PT** — Nhà phát triển PTBot 🇻🇳

---

## 📄 Giấy phép

Dự án cá nhân — vui lòng giữ phần ghi công tác giả **PT** khi sử dụng hoặc phát triển tiếp.
