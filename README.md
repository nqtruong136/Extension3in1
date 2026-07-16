# 🚀 WebBrain Ultimate Edition (3-In-1)

Chào mừng ní đến với phiên bản **WebBrain Ultimate Edition (3-trong-1)** tích hợp hoàn chỉnh và tối ưu hóa tối đa. Gói cài đặt này hợp nhất 3 cấu phần quan trọng vào một thư mục sạch sẽ duy nhất:
1.  **AI Browser Agent gốc (WebBrain Chat)**: Trò chuyện trực tiếp với trang web, hỗ trợ đa LLM.
2.  **Model Context Protocol (MCP) Server**: Kết nối WebSocket thời gian thực (Port `8545`) điều phối 27 công cụ duyệt web và đặc quyền gỡ lỗi từ xa (CDP).
3.  **Stealth Shopee Crawler**: Tự động cào dữ liệu Shopee vượt CAPTCHA, kết hợp quét ảnh CDN/video độ phân giải cao và lưu trữ local.

---

## 📂 Cấu Trúc Thư Mục Đóng Gói
```text
3in1/
├── extension/          # Mã nguồn Chrome Extension đã tích hợp Tab Bar & 3 luồng hoạt động
├── server/             # WebSocket MCP Server (đã thêm tool upload_file, loại bỏ node_modules)
└── scripts/            # Script Node.js tải tài nguyên local tự động
```

---

## 🛠️ Hướng Dẫn Cài Đặt & Vận Hành

### Bước 1: Nạp Chrome Extension
1.  Mở trình duyệt Google Chrome và truy cập: `chrome://extensions/`
2.  Bật **Chế độ dành cho nhà phát triển (Developer mode)** ở góc trên bên phải.
3.  Nhấp vào **Tải tiện ích đã giải nén (Load unpacked)** ở góc trên bên trái.
4.  Chọn thư mục con: `g:\03_BAITAP\browser-use-agent\3in1\extension`
5.  Extension WebBrain Ultimate sẽ xuất hiện. Nhấp vào icon Tiện ích trên thanh công cụ Chrome để mở **Sidebar panel**.

### Bước 2: Khởi Chạy Local MCP Server
1.  Mở Terminal (cmd hoặc powershell) tại thư mục `server`:
    ```bash
    cd g:\03_BAITAP\browser-use-agent\3in1\server
    ```
2.  Cài đặt các gói phụ thuộc cần thiết (MCP SDK & ws):
    ```bash
    npm install
    ```
3.  Khởi chạy Server:
    ```bash
    npm start
    # Hoặc: node index.js
    ```
    *Server sẽ chạy và lắng nghe kết nối WebSocket tại `ws://localhost:8545`.*
4.  **Kiểm tra:** Quay lại Sidebar trên Chrome, nhấp vào tab **MCP Server**. Bạn sẽ thấy chấm trạng thái chuyển sang màu xanh lá cây 🟢 **CONNECTED** báo hiệu kết nối thành công!

### Bước 3: Vận Hành Shopee Crawler & Tải Tài Nguyên
1.  Chọn tab **Shopee Crawler** trên Sidebar.
2.  Nhập các URL sản phẩm Shopee cần cào dữ liệu.
3.  Điền API Key Gemini (hoặc OpenAI compatible endpoint) và nhấn **Bắt đầu Crawl**.
4.  **Tải ảnh & video về máy local:** Sau khi cào xong, xuất file kết quả JSON (ví dụ: `shopee_scraped.json`). Mở terminal và chạy script tải ảnh/video tự động:
    ```bash
    node g:\03_BAITAP\browser-use-agent\3in1\scripts\download-media.js "C:/path/to/your/shopee_scraped.json"
    ```
    *Toàn bộ ảnh CDN và video gốc sẽ được tự động lưu về thư mục `3in1/downloads/` phân chia rõ ràng theo từng tên sản phẩm.*

---

## 🌟 Các Tính Năng & Cải Tiến Đã Được Tích Hợp

*   **Hiệu Ứng Viền Sáng Mờ Ảo MCP (Debounced Glow):** Viền gradient nhấp nháy sẽ duy trì liên tục trong suốt chuỗi hành động gõ chữ/click của Agent và chỉ tự động tắt đi sau khi Agent dừng hoạt động được 8 giây (tránh việc bật/tắt chập chờn sau mỗi 1.2 giây như bản gốc).
*   **Mở Tab Cào Thông Minh:** Tab cào Shopee mới sẽ tự động được tạo ngay bên cạnh tab hiện tại (trong cùng Window) và đưa vào cùng một **Nhóm tab (tabGroup)** nếu ní đang sử dụng nhóm tab, giúp trình duyệt cực kỳ gọn gàng.
*   **Không Tự Đóng Tab Khi Xong:** Tab cào cuối cùng sẽ được giữ nguyên và viền chuyển sang **màu xanh dương** báo hiệu hoàn tất thay vì tự động tắt đi để ní dễ dàng kiểm tra kết quả.
*   **Giải Nghĩa 27 Công Cụ MCP bằng Tiếng Việt:** Click vào bất kỳ công cụ nào trong tab MCP Server ở Sidebar để đọc ngay mô tả chi tiết, dễ hiểu về khái niệm và cách thức hoạt động của công cụ đó.
*   **Hỗ Trợ Upload File CDP (`upload_file`):** Tích hợp công cụ upload file đặc quyền vào cả MCP Server và Chrome Extension sử dụng API `chrome.debugger` để bypass sandbox bảo mật của Chrome.
*   **Lưu Trạng Thái Tab Hoạt Động:** Tự động ghi nhớ tab giao diện ní đang mở dở (Chat / MCP / Crawler) khi ní đóng/mở lại Sidebar.

---

## 💖 Lời Cảm Ơn & Nguồn Gốc (Credits)

Dự án WebBrain Ultimate Edition này được phát triển, tùy biến và hợp nhất dựa trên các mã nguồn mở tuyệt vời của cộng đồng. Chúng mình xin gửi lời cảm ơn và sự trân trọng sâu sắc tới các tác giả gốc:

1.  **Dự án WebBrain (AI Browser Agent)**:
    *   **Tác giả/Tổ chức**: [webbrain-one](https://github.com/webbrain-one/webbrain)
    *   **Đóng góp**: Cung cấp toàn bộ nền tảng mã nguồn Chrome Extension gốc, giao diện Sidebar mượt mà, công nghệ trích xuất Accessibility Tree và các mô-đun AI Agent xuất sắc.
2.  **Mã nguồn MCP Server Fork**:
    *   **Đóng góp**: Cung cấp cấu trúc WebSocket kết nối chuẩn MCP SDK để điều khiển trình duyệt từ bên ngoài.

*Sự sáng tạo và đóng góp mở của các tác giả là nền tảng cốt lõi giúp tụi mình có thể tùy biến và xây dựng nên phiên bản Ultimate 3-trong-1 mạnh mẽ này!*

