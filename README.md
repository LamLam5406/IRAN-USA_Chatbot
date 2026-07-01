# Trợ Lý Ảo Phân Tích Địa Chính Trị: Xung Đột Iran - Mỹ (2025-2026)

Dự án này xây dựng một hệ thống **Hỏi đáp thông minh (RAG - Retrieval-Augmented Generation)** chuyên biệt để phân tích các sự kiện địa chính trị liên quan đến xung đột giữa Iran và Mỹ trong giai đoạn 2025-2026.

Hệ thống sử dụng sức mạnh của mô hình ngôn ngữ lớn (LLM) kết hợp với cơ sở dữ liệu vector để tra cứu, tổng hợp thông tin từ hàng ngàn bài báo và đưa ra câu trả lời chính xác, bám sát dữ liệu thực tế.

## Tính Năng Chính

* **Xử lý dữ liệu văn bản:** Tự động tải, làm sạch và làm giàu (enrich) dữ liệu từ các bài báo (bao gồm Nguồn, Ngày tháng, Tiêu đề và Nội dung).
* **Lưu trữ Vector cục bộ:** Sử dụng mô hình nhúng (Embedding) nhẹ và tối ưu của HuggingFace để chuyển đổi văn bản và lưu trữ dài hạn trong ChromaDB.
* **Luồng RAG cốt lõi:** Kết hợp LangChain và Google Gemini API (phiên bản `gemini-3.5-flash`) để lấy ra top 10 đoạn thông tin liên quan nhất và sinh ra câu trả lời tự nhiên.
* **Giao diện Chatbot Trực quan:** Triển khai giao diện người dùng web bằng Gradio, cho phép tương tác với trợ lý ảo trực tiếp qua trình duyệt với các xử lý báo lỗi API mượt mà.

## Công Nghệ Sử Dụng

* **Ngôn ngữ:** Python
* **Framework LLM:** [LangChain](https://python.langchain.com/) (bao gồm `langchain-community`, `langchain-google-genai`, `langchain-huggingface`)
* **Mô hình ngôn ngữ (LLM):** Google Gemini (`gemini-3.5-flash`)
* **Mô hình Embedding:** HuggingFace (`all-MiniLM-L6-v2`)
* **Cơ sở dữ liệu Vector:** [ChromaDB](https://www.trychroma.com/)
* **Giao diện người dùng:** [Gradio](https://www.gradio.app/)
* **Xử lý dữ liệu:** Pandas

---

## Hướng Dẫn Cài Đặt Và Chạy Dự Án

Dự án này được thiết kế để chạy mượt mà trên **Google Colab**. Dưới đây là các bước để bạn triển khai:

### 1. Chuẩn Bị Môi Trường

* Tạo một thư mục trên Google Drive cá nhân, ví dụ: `MyDrive/Projects/IRAN_USA/`.
* Tải tệp dữ liệu bài báo định dạng CSV (ví dụ: `iran_us_news_dataset.csv`) lên thư mục này.
* Thiết lập **API Key** của Google Gemini. Trong Google Colab, hãy thêm Secret Key với tên `GEMINI_API_KEY`.

### 2. Kiến Trúc Notebook

Notebook được chia thành 5 phần chính, bạn có thể chạy lần lượt từng ô mã (cell):

1. **Cài đặt thư viện và thiết lập API:** Cài đặt LangChain, các thư viện liên quan và cấu hình Gemini API.
2. **Tải, Làm sạch và Chuẩn bị Dữ liệu:** Mount Google Drive, đọc tệp CSV, lọc bỏ dữ liệu rỗng và ghép các thông tin siêu dữ liệu (metadata) vào nội dung bài viết để tăng cường ngữ cảnh.
3. **Cắt nhỏ văn bản (Chunking) & Tạo Vector Database:**
* Sử dụng `RecursiveCharacterTextSplitter` (chunk_size=1000, chunk_overlap=200).
* Mã hóa bằng mô hình `all-MiniLM-L6-v2` (nhẹ, chạy tốt trên CPU).
* Khởi tạo và lưu ChromaDB vào Google Drive để tái sử dụng mà không cần huấn luyện lại.


4. **Tạo Luồng Hỏi Đáp (RAG Chain):** Thiết lập kết nối giữa Retriever (truy xuất 10 đoạn văn bản gần nhất), Prompt chuyên gia địa chính trị, và mô hình Gemini để xử lý câu hỏi.
5. **Tạo Giao Diện Chat Trực Quan với Gradio:** Khởi chạy một UI ứng dụng web để chat trực tiếp với RAG Bot. Có sẵn link Public share để gửi cho người khác trải nghiệm.

### 3. Cấu Hình Đường Dẫn (Lưu ý quan trọng)

Nếu bạn sao chép (clone) dự án này, hãy đảm bảo thay đổi các đường dẫn thư mục trong mã nguồn cho khớp với Google Drive của bạn:

* Đường dẫn tệp CSV: `/content/drive/MyDrive/Projects/IRAN_USA/iran_us_news_dataset.csv`
* Đường dẫn lưu ChromaDB: `/content/drive/MyDrive/Projects/IRAN_USA/chroma_db_storage`

---

## Ví Dụ Tương Tác

Khi giao diện Gradio được khởi chạy thành công, bạn có thể thử một số câu hỏi (prompt) như sau:

* *"Chiến dịch Midnight Hammer là gì và diễn ra khi nào?"*
* *"Chuyện gì đã xảy ra tại cơ sở hạt nhân Fordow?"*
* *"Fox News và Al Jazeera đưa tin khác nhau thế nào về cuộc không kích?"*

---

## Khuyến Cáo

* Bot được chỉ định trả lời nghiêm ngặt dựa trên dữ liệu từ file CSV cung cấp. Nếu thông tin không có trong cơ sở dữ liệu, Bot sẽ phản hồi là không biết để tránh hội chứng "ảo giác" (Hallucination) của AI.
* Thông tin được cung cấp bởi trợ lý mang tính tham khảo và phục vụ cho mục đích nghiên cứu dữ liệu tin tức. Nên cân nhắc đối chiếu thực tế trước khi sử dụng.
