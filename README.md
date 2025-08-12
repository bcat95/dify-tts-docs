# 🗣️ Hướng dẫn API Văn bản thành Giọng Nói - Dify (Tiếng Việt)

## 1. Giới thiệu
API **Văn bản thành Giọng nói (Text-to-Speech - TTS)** của Dify cho phép bạn gửi văn bản và nhận lại giọng nói dưới dạng file âm thanh.  
Có thể kết hợp với Workflow để xử lý dữ liệu, dịch thuật, đọc văn bản, hoặc tạo chatbot có giọng nói.

- **Base URL:** `http://dify.chatvn.org/v1`
- **Xác thực:** Sử dụng API Key:
  ```text
  Authorization: Bearer {API_KEY}
---

## 2. Chạy Workflow (`POST /workflows/run`)

Dùng để chạy một Workflow đã được publish.

**Endpoint:**

```text
POST /workflows/run
```

**Body:**

```json
{
  "inputs": {
    "text": "Xin chào, đây là thử nghiệm TTS"
  },
  "response_mode": "streaming",
  "user": "abc-123"
}
```

**Tham số quan trọng:**

* `inputs` – Các biến đầu vào của workflow (ít nhất 1 biến)
* `response_mode` – `streaming` (khuyến nghị) hoặc `blocking`
* `user` – ID người dùng cuối (do bạn đặt)

**Ví dụ Curl:**

```bash
curl -X POST 'http://dify.chatvn.org/v1/workflows/run' \
  --header 'Authorization: Bearer {api_key}' \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "inputs": {"text": "Xin chào thế giới"},
    "response_mode": "streaming",
    "user": "abc-123"
  }'
```

---

## 3. Chạy Workflow cụ thể (`POST /workflows/:workflow_id/run`)

Khi muốn chỉ định một workflow version cụ thể:

```bash
curl -X POST 'http://dify.chatvn.org/v1/workflows/{workflow_id}/run' \
  --header 'Authorization: Bearer {api_key}' \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "inputs": {"text": "Xin chào"},
    "response_mode": "streaming",
    "user": "abc-123"
  }'
```

---

## 4. Upload file (`POST /files/upload`)

Cho phép tải file lên để workflow xử lý (văn bản, hình ảnh, âm thanh, video...).

**Request:**

```bash
curl -X POST 'http://dify.chatvn.org/v1/files/upload' \
  --header 'Authorization: Bearer {api_key}' \
  --form 'file=@localfile.txt;type=text/plain' \
  --form 'user=abc-123'
```

**Response:**

```json
{
  "id": "72fa9618-8f89-4a37-9b33-7e1178a24a67",
  "name": "example.txt",
  "size": 1024,
  "extension": "txt",
  "mime_type": "text/plain"
}
```

---

## 5. Lấy chi tiết Workflow đang chạy (`GET /workflows/run/:workflow_run_id`)

```bash
curl -X GET 'http://dify.chatvn.org/v1/workflows/run/{workflow_run_id}' \
  -H 'Authorization: Bearer {api_key}'
```

---

## 6. Dừng Workflow đang chạy (`POST /workflows/tasks/:task_id/stop`)

Chỉ áp dụng với **streaming mode**:

```bash
curl -X POST 'http://dify.chatvn.org/v1/workflows/tasks/{task_id}/stop' \
  -H 'Authorization: Bearer {api_key}' \
  --data-raw '{"user": "abc-123"}'
```

---

## 7. Xem file đã upload (`GET /files/:file_id/preview`)

**Xem hoặc tải file về:**

```bash
curl -X GET 'http://dify.chatvn.org/v1/files/{file_id}/preview' \
  -H 'Authorization: Bearer {api_key}'
```

**Tải file dạng attachment:**

```bash
curl -X GET 'http://dify.chatvn.org/v1/files/{file_id}/preview?as_attachment=true' \
  -H 'Authorization: Bearer {api_key}' \
  --output myfile.png
```

---

## 8. Xem logs workflow (`GET /workflows/logs`)

```bash
curl -X GET 'http://dify.chatvn.org/v1/workflows/logs' \
  -H 'Authorization: Bearer {api_key}'
```

---

## 9. Lấy thông tin ứng dụng (`GET /info`)

```bash
curl -X GET 'http://dify.chatvn.org/v1/info' \
  -H 'Authorization: Bearer {api_key}'
```

---

## 10. Lấy cấu hình tham số (`GET /parameters`)

```bash
curl -X GET 'http://dify.chatvn.org/v1/parameters'
```

---

## 11. Lấy cài đặt WebApp (`GET /site`)

```bash
curl -X GET 'http://dify.chatvn.org/v1/site' \
  -H 'Authorization: Bearer {api_key}'
```

---

## 12. Mã lỗi thường gặp

| Mã lỗi                        | Mô tả                   |
| ----------------------------- | ----------------------- |
| 400 invalid\_param            | Tham số không hợp lệ    |
| 400 app\_unavailable          | Ứng dụng không khả dụng |
| 400 provider\_quota\_exceeded | Hết quota gọi model     |
| 400 workflow\_not\_found      | Không tìm thấy workflow |
| 500 internal server error     | Lỗi máy chủ             |

---

## 13. Ghi chú khi dùng TTS

* **streaming** sẽ nhận dữ liệu âm thanh theo từng chunk (`tts_message`) → cần decode base64 để phát.
* **blocking** sẽ chờ xử lý xong mới trả kết quả.
* API Key cần được bảo mật (không nhúng trực tiếp vào client).

---

## 14. Tài nguyên tham khảo

* Trang chủ Dify: [https://dify.ai](https://dify.ai)
* Docs chính thức: [https://docs.dify.ai](https://docs.dify.ai)
