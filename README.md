# 🚀 WebBrain Ultimate Edition (3-In-1)

Chào mừng bạn đến với **WebBrain Ultimate Edition (3-trong-1)** - giải pháp AI Browser Agent tích hợp hoàn chỉnh và tối ưu hóa tối đa. Gói cài đặt này hợp nhất 3 cấu phần quan trọng vào một thư mục phân phối duy nhất, được kế thừa và phát triển từ các mã nguồn mở chất lượng cao của cộng đồng.

---

## 📂 Cấu Trúc Gói Đóng Gói
```text
3in1/
├── extension/          # Chrome Extension tích hợp Tab Bar & 3 luồng giao diện hoạt động
├── server/             # WebSocket MCP Server (đã tích hợp upload_file qua CDP, không chứa node_modules)
└── scripts/            # Script Node.js tối giản tự động tải tài nguyên local
```

---

## 🛠️ Hướng Dẫn Cài Đặt & Vận Hành Nhanh

### Bước 1: Nạp Chrome Extension
1.  Truy cập trang quản lý tiện ích: `chrome://extensions/` trên Google Chrome.
2.  Kích hoạt **Chế độ dành cho nhà phát triển (Developer mode)** ở góc trên cùng bên phải.
3.  Nhấp vào nút **Tải tiện ích đã giải nén (Load unpacked)** ở góc trên bên trái.
4.  Chọn đường dẫn thư mục: `3in1/extension`
5.  Mở Sidebar của WebBrain để bắt đầu sử dụng.

### Bước 2: Cài Đặt & Cấu Hình MCP Server Cho Các IDE / AI Agents

Server WebBrain hoạt động theo cơ chế giao tiếp kép:
- **WebSocket (`ws://localhost:8545`)**: Kết nối và điều khiển trực tiếp Chrome Extension.
- **STDIO (Standard Input/Output)**: Kết nối với các IDE và AI Agent (Antigravity, Claude Code, Codex, Cursor, Windsurf, Claude Desktop,...).

#### 1. Cài đặt phụ thuộc:
Mở Terminal tại thư mục `server` và cài đặt các thư viện:
```bash
cd 3in1/server
npm install
```

#### 2. Thêm MCP Server vào các IDE / AI Agent:

* **Antigravity / VS Code / Cursor / Windsurf / Codex / Claude Desktop** (Dùng tệp cấu hình JSON `mcp.json` hoặc `claude_desktop_config.json`):
  ```json
  {
    "mcpServers": {
      "webbrain": {
        "command": "node",
        "args": ["C:/path/to/3in1/server/index.js"]
      }
    }
  }
  ```
  *(Thay `C:/path/to/3in1/server/index.js` bằng đường dẫn tuyệt đối đến file `index.js` trong thư mục `server` trên máy của bạn).*

* **Claude Code (CLI)**:
  Chạy lệnh đăng ký MCP server trực tiếp trong Terminal:
  ```bash
  claude mcp add webbrain -- node C:/path/to/3in1/server/index.js
  ```

> 💡 **Lưu ý quan trọng:** Khi bạn đã thêm cấu hình `mcpServers` vào IDE hoặc chạy lệnh `claude mcp add`, IDE / AI Agent sẽ **tự động kích hoạt** MCP Server (`node index.js`) ngầm mỗi khi khởi động. Do đó, bạn **KHÔNG CẦN** chạy lệnh `npm start` thủ công trong Terminal nữa để tránh bị đụng cổng WebSocket `8545` (`EADDRINUSE`).

* **Khởi chạy thủ công (Chỉ dùng khi Dev / Debug không qua IDE)**:
  ```bash
  cd 3in1/server
  npm start
  ```
  *(Chỉ mở khi test trực tiếp từ terminal mà không khai báo file `mcpServers` trong IDE).*

#### 3. Kiểm tra kết nối:
1. **Phía Trình duyệt:** Quay lại Sidebar Extension ➔ Tab **MCP Server** ➔ Trạng thái hiển thị 🟢 **CONNECTED** (kết nối thành công cổng WebSocket `ws://localhost:8545`).
2. **Phía IDE / Agent:** IDE sẽ tự động gọi `node index.js` qua STDIO và nhận diện được 28 công cụ MCP của `webbrain` (ví dụ: `get_accessibility_tree`, `click_ax`, `upload_file`,...).

### Bước 3: Vận Hành Shopee Crawler & Tải Tài Nguyên
1.  Chọn tab **Shopee Crawler** trên Sidebar.
2.  Dán danh sách các link sản phẩm Shopee cần cào.
3.  Nhập API Key Gemini (hoặc OpenAI Endpoint) ➔ Nhấn **Bắt đầu Crawl**.
4.  Khi hoàn tất, xuất tệp kết quả JSON (ví dụ: `shopee_scraped.json`). Mở terminal và chạy lệnh tải toàn bộ ảnh/video gốc về máy:
    ```bash
    node ../scripts/download-media.js "C:/path/to/your/shopee_scraped.json"
    ```
    *Ảnh CDN và video giới thiệu sẽ được lưu tự động trong thư mục `3in1/downloads/` theo từng thư mục tên sản phẩm.*

---

## 📋 Chi Tiết Các Công Cụ Kỹ Thuật Theo Từng Luồng Hoạt Động

Hệ thống hoạt động song song trên 3 luồng độc lập, cung cấp bộ công cụ từ tương tác ngữ cảnh thô đến can thiệp sâu hệ thống:

### 1. Luồng AI Browser Agent (Tab 1 - WebBrain Chat)
Hoạt động như một trợ lý hội thoại trực tiếp với nội dung trang web hiện tại thông qua các đề xuất hành động (Recommended Actions):
*   **Chat with Page:** Phân tích ngữ cảnh, trả lời câu hỏi dựa trên văn bản hiển thị của tab đang hoạt động.
*   **Page Summarization:** Tóm tắt nhanh nội dung bài viết, tài liệu nghiên cứu dài.
*   **Task Planning:** Phác thảo kế hoạch và tự động hóa các thao tác cơ bản dựa trên prompt tự nhiên của người dùng.

### 2. Luồng Model Context Protocol (Tab 2 - WebSocket MCP Server)
Xuất bản **28 công cụ hệ thống** ra WebSocket Server ở cổng `8545` để các Agent bên ngoài (như Claude, Gemini, IDE) kết nối và điều phối trình duyệt từ xa. Danh sách công cụ bao gồm:

*   **Nhóm Tương Tác DOM Chính Xác (Accessibility Tree):**
    *   `get_accessibility_tree`: Đọc cấu trúc cây trợ năng của trang hiện tại, tạo ra các mã định danh `ref_id` duy nhất và ổn định cho mỗi phần tử.
    *   `click_ax`: Bấm chính xác tuyệt đối vào phần tử DOM bằng `ref_id` (tránh lệch tọa độ hoặc lỗi cuộn trang).
    *   `type_ax`: Gõ văn bản vào ô nhập liệu bằng `ref_id`.
    *   `set_field`: Điền văn bản nhanh và tùy chọn nhấn phím Enter (kết hợp click + type).
    *   `hover`: Di chuyển con trỏ chuột ảo lên trên phần tử bằng `ref_id`.
    *   `get_selection`: Trích xuất nhanh đoạn văn bản đang được người dùng bôi đen.
*   **Nhóm Điều Hướng & Trạng Thế Mạng:**
    *   `navigate`: Chuyển hướng tab đến một liên kết URL đích cụ thể.
    *   `new_tab`: Tạo một tab mới và tự động nhóm vào phân vùng WebBrain.
    *   `go_back` / `go_forward`: Quay lại hoặc đi tiếp trong lịch sử duyệt tab.
    *   `wait_for_stable`: Chờ mạng lưới tĩnh lặng và trang web hoàn tất kết xuất (render).
*   **Nhóm Thao Tác Cửa Sổ & Giao Diện:**
    *   `resize_window`: Thay đổi kích thước cửa sổ hiển thị của Chrome.
    *   `get_window_info`: Lấy thông tin kích thước và danh sách tab hiện tại.
    *   `screenshot`: Chụp ảnh màn hình khu vực hiển thị (Viewport) của trang.
*   **Nhóm Quản Lý Tệp & Tải Xuống:**
    *   `list_downloads`: Tìm kiếm lịch sử các tệp tin đã tải xuống.
    *   `download_files`: Tải tệp từ liên kết URL trực tiếp thông qua trình quản lý tải xuống của Chrome.
    *   `upload_file` *(Đặc Quyền CDP)*: Sử dụng API `chrome.debugger` gắn CDP để gửi lệnh `DOM.setFileInputFiles` đưa các file local từ máy lên trang web (bypass hoàn toàn sandbox bảo mật).
*   **Nhóm Thao Tác DOM Dự Phòng (CSS Selector):**
    *   `click` / `type_text`: Thực hiện click và gõ chữ dựa trên CSS selector truyền thống hoặc index khi `ref_id` không tồn tại.
    *   `wait_for_element`: Đợi cho đến khi phần tử khớp với CSS Selector xuất hiện trên trang.
    *   `get_interactive_elements`: Quét nhanh các phần tử tương tác thô kèm chỉ mục index trên DOM.
    *   `extract_data`: Trích xuất nhanh dữ liệu có cấu trúc như bảng biểu hoặc link.
    *   `read_page` / `read_page_source`: Đọc văn bản thô hiển thị hoặc lấy toàn bộ mã nguồn HTML (`outerHTML`).
*   **Nhóm Thực Thi Nâng Cao:**
    *   `execute_js`: Khởi chạy và thực thi một khối mã JavaScript bất đồng bộ động trong ngữ cảnh của trang (nhận tham số callback `resolve`).

### 3. Luồng Shopee Crawler (Tab 3 - Stealth Crawler)
Vận hành một hệ thống cào dữ liệu Shopee có cấu trúc, tích hợp các cơ chế mô phỏng sinh học để tránh bị chặn:
*   **Stealth Humanize:** Tự động giả lập hành vi cuộn chuột tự nhiên (Wave scroll) và di chuyển con trỏ chuột ngẫu nhiên trên trang để vượt qua các thuật toán phát hiện bot.
*   **AI Data Parsing:** Chụp ảnh màn hình kết hợp gửi text thô đến các LLM (Gemini, DeepSeek) để phân tích và trả về cấu trúc JSON chuẩn gồm 6 trường dữ liệu: *Name, Price, Sold, Rating, Brand, Description*.
*   **Media Link Extraction:** Tự động chạy mã JavaScript cô lập để nhặt toàn bộ mảng link ảnh CDN chất lượng cao của sản phẩm (`Images`) và đường dẫn video sản phẩm (`Video`).

---

## 🌟 Các Điểm Cải Tiến Độc Quyền Trong Bản Ultimate

1.  **Hiệu ứng viền nhấp nháy liên tục (Debounced Glow Border):** Khi chạy các tác vụ MCP, viền sáng gradient tím bao quanh tab sẽ liên tục nhấp nháy và tự động duy trì. Viền chỉ tắt đi sau khi hệ thống hoàn tất toàn bộ chuỗi công việc và nhàn rỗi (idle) trong 8 giây.
2.  **Mở tab thông minh đưa vào nhóm tab:** Khi bấm cào Shopee, tab cào mới sẽ được tạo ngay bên cạnh tab hiện tại. Nếu tab hiện tại nằm trong một nhóm tab (tabGroup), tab cào sẽ được tự động đưa vào chung nhóm đó để giữ trình duyệt ngăn nắp.
3.  **Không tự đóng tab cào:** Giữ nguyên tab cào ở trang sản phẩm cuối cùng sau khi hoàn tất, đồng thời đổi viền sáng sang màu xanh dương báo hiệu thành công.
4.  **Giải nghĩa Tiếng Việt trực quan:** Tích hợp bộ giải nghĩa khái niệm và công dụng cho 28 công cụ MCP ngay trên giao diện Sidebar.

---

## 💖 Lời Cảm Ơn & Nguồn Gốc (Credits)

Dự án **WebBrain Ultimate Edition (3-In-1)** này được phát triển, tùy biến và đóng gói dựa trên các đóng góp mở tuyệt vời của cộng đồng. Chúng tôi xin gửi lời tri ân sâu sắc tới:

*   **Dự Án Gốc WebBrain**: Phát triển bởi [webbrain-one](https://github.com/webbrain-one/webbrain) - nền tảng Chrome Extension AI Agent xuất sắc.
*   **Bản Fork WebBrain MCP của tác giả**: Được lưu trữ và đóng góp bởi [nqtruong136/webbrain](https://github.com/nqtruong136/webbrain) - cung cấp nền móng kết nối WebSocket MCP Server đầu tiên.

*Mã nguồn mở là cầu nối sáng tạo, xin chân thành cảm ơn các đóng góp của các tác giả!*
