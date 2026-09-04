`````markdown
# 🤖 PTBot — Trợ lý AI tiếng Việt

![Version](https://img.shields.io/badge/PTBot-v5.0%20Elite-7c5cff)
![Single File](https://img.shields.io/badge/Single-File%20HTML-5b8def)
![No Build](https://img.shields.io/badge/No%20Build-Required-4ade80)

> Ứng dụng chatbot AI hoàn chỉnh gói gọn trong **một file HTML duy nhất** — không cần cài đặt, không cần build, không dependency.
> Được tạo ra và phát triển bởi **PT**.

PTBot là trợ lý AI tiếng Việt với giao diện dark mode hiện đại, đặc biệt mạnh về **lập trình** và **sáng tạo nội dung**. Toàn bộ dữ liệu hội thoại được lưu **cục bộ trên trình duyệt** của bạn.

---

## ✨ Tính năng

- 💬 **Giao diện chat hiện đại** — dark mode, hiệu ứng gradient, animation mượt mà
- 🗂️ **Quản lý nhiều cuộc trò chuyện** — tạo mới, chuyển đổi, xóa từng cuộc hoặc xóa tất cả
- 💾 **Lưu trữ cục bộ** — hội thoại lưu vào `localStorage`, tự khôi phục khi mở lại
- 📝 **Render Markdown đầy đủ** — in đậm, in nghiêng, tiêu đề, danh sách, trích dẫn, code inline
- 💻 **Code block cao cấp** — hiển thị ngôn ngữ, nút **Sao chép** một click
- 🔁 **Tạo lại & Thử lại** — regenerate câu trả lời, retry khi lỗi
- 🖥️ **Responsive** — sidebar dạng drawer với overlay khi dùng mobile
- ⌨️ **Phím tắt** — `Enter` gửi, `Shift+Enter` xuống dòng, `/` focus ô nhập
- 🧩 **Persona tùy biến** — system prompt định danh PTBot, lọc phản hồi giữ persona nhất quán
- 🔔 **Toast thông báo** + hiển thị lỗi chi tiết kèm mã lỗi

## 🖼️ Demo

> Mở trực tiếp file `chatbot.html` trên trình duyệt để trải nghiệm.
> *(Bạn có thể thêm screenshot tại đây)*

---

## 🚀 Bắt đầu nhanh

### Yêu cầu

- Trình duyệt hiện đại (Chrome, Edge, Firefox, Safari)
- API key của một nhà cung cấp AI **tương thích chuẩn OpenAI**

### Các bước

1. Tải file `chatbot.html` về máy (hoặc clone repository):

   ```bash
   git clone https://github.com/[username]/[repo].git
   ```

2. **Cấu hình API** — mở file, tìm khối `API_CONFIG` ở đầu `<script>` (xem [mục bên dưới](#%EF%B8%8F-cấu-hình-api)).

3. Mở bằng trình duyệt:

   ```bash
   # Nhấn đúp file, hoặc chạy local server (khuyến nghị để tránh lỗi CORS):
   python -m http.server 8000
   # rồi truy cập http://localhost:8000/chatbot.html
   ```

4. Chào PTBot và bắt đầu trò chuyện! 🎉

---

## ⚙️ Cấu hình API

Mở `chatbot.html`, tìm đoạn sau ở đầu thẻ `<script>`:

```javascript
const API_CONFIG = {
  baseUrl: 'Your base url',   // 🔗 Endpoint gốc (không có /chat/completions)
  apiKey: 'Your api key',     // 🔑 API key của bạn
  model: 'Your model'         // 🤖 Tên model
};
```

Ví dụ với các nhà cung cấp phổ biến (bất kỳ API tương thích chuẩn OpenAI đều dùng được):

| Nhà cung cấp | baseUrl | model |
|---|---|---|
| OpenAI | `https://api.openai.com/v1` | `gpt-4o-mini` |
| DeepSeek | `https://api.deepseek.com/v1` | `deepseek-chat` |
| Groq | `https://api.groq.com/openai/v1` | `llama-3.3-70b-versatile` |
| OpenRouter | `https://openrouter.ai/api/v1` | `[tùy chọn]` |
| Ollama (local) | `http://localhost:11434/v1` | `llama3` |

> ⚠️ **Lưu ý bảo mật:** API key nằm trong file HTML phía client. **Không** commit key thật lên repository công khai và không deploy công khai với key nhúng sẵn. Với production, hãy chuyển sang server proxy.

---

## ⌨️ Phím tắt

| Phím | Chức năng |
|---|---|
| `Enter` | Gửi tin nhắn |
| `Shift + Enter` | Xuống dòng |
| `/` | Focus vào ô nhập liệu |

---

## 🧠 Cách hoạt động

| Thành phần | Chi tiết |
|---|---|
| **Endpoint** | `POST {baseUrl}/chat/completions` (chuẩn OpenAI) |
| **Tham số** | `temperature: 0.7` · `max_tokens: 3000` · không stream |
| **Context** | Gửi 20 tin nhắn gần nhất + system prompt persona |
| **Persona** | `PTBOT_PERSONA` + `PTBOT_REINFORCE` — định danh PTBot, giọng điệu tiếng Việt |
| **Bộ lọc phản hồi** | `sanitizeResponse()` loại bỏ các câu làm lệch persona |
| **Lưu trữ** | `localStorage` — key `ptbot_conversations` (dữ liệu chỉ nằm trên máy bạn) |
| **Tiêu đề hội thoại** | Tự động lấy 40 ký tự đầu của tin nhắn đầu tiên |

---

## 🎨 Tùy biến

- **Đổi persona / giọng điệu bot:** sửa chuỗi `PTBOT_PERSONA` trong `<script>`
- **Đổi màu chủ đạo:** sửa biến CSS trong khối `:root` (ví dụ `--accent-primary: #7c5cff`)
- **Đổi tên & version:** sửa logo `PT`, nhãn `v5.0 Elite`, dòng status trong sidebar
- **Đổi thẻ gợi ý:** sửa 4 `suggestion-card` trong hàm `getWelcomeHTML()`
- **Đổi tham số AI:** sửa `temperature` / `max_tokens` trong hàm `callAPI()`
- **Đổi số tin nhắn context:** sửa `conv.messages.slice(-20)` trong `getAIResponse()`

## 📁 Cấu trúc project

```
.
├── chatbot.html    # Toàn bộ ứng dụng: HTML + CSS + JavaScript
└── README.md
```

---

## ❓ Xử lý sự cố

| Vấn đề | Cách xử lý |
|---|---|
| **Lỗi mạng / CORS** | Chạy qua local server (`python -m http.server`) thay vì mở `file://` |
| **"API key không hợp lệ"** | Kiểm tra lại `apiKey` trong `API_CONFIG` |
| **"Quá nhiều yêu cầu"** | Bị rate limit (429) — đợi ít phút rồi thử lại |
| **"Server đang gặp sự cố"** | Lỗi phía nhà cung cấp (5xx) — bấm nút *Thử lại* |
| **Phản hồi rỗng** | Model không trả nội dung — bấm *Tạo lại* |
| **Sao chép không hoạt động** | API Clipboard cần HTTPS/`localhost` — app có fallback tự động |

---

## 📄 License

Distributed under the MIT License. Xem file `LICENSE` để biết chi tiết.

## 👤 Tác giả

**PT** — Nhà phát triển PTBot

> PTBot v5.0 · Siêu thông minh 🟢
`````

---

### 📌 Hướng dẫn sử dụng nhanh:

1. Tạo file mới tên `README.md` trong thư mục chứa `chatbot.html`
2. Copy toàn bộ nội dung ở trên vào file
3. (Tùy chọn) Thay `[username]/[repo]` bằng link GitHub thật của bạn
4. Save lại — GitHub sẽ tự động hiển thị README đẹp ngay trên trang repo

Nếu bạn cần tôi thêm mục **Deploy** (hướng dẫn đưa lên GitHub Pages/Vercel) hoặc rút gọn README, cứ nói nhé!
