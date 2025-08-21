# Patent AI Agent - Keyword Extraction System

Một hệ thống AI agent chuyên dụng cho việc trích xuất từ khóa bằng sáng chế với kiến trúc module hóa và dễ bảo trì.

## 🏗️ Kiến Trúc Dự Án

```
priorart_p/
├── src/                           # Mã nguồn chính
│   ├── core/                      # Module chính của AI agent
│   │   ├── __init__.py
│   │   └── extractor.py           # Lớp CoreConceptExtractor chính
│   ├── api/                       # Tích hợp API bên ngoài
│   │   ├── __init__.py
│   │   └── ipc_classifier.py      # API phân loại IPC/CPC
│   ├── crawling/                  # Thu thập dữ liệu web
│   │   ├── __init__.py
│   │   └── patent_crawler.py      # Crawler cho Google Patents
│   ├── evaluation/                # Đánh giá và so sánh
│   │   ├── __init__.py
│   │   └── similarity_evaluator.py # Đánh giá độ tương tự
│   ├── prompts/                   # Quản lý prompt templates
│   │   ├── __init__.py
│   │   └── extraction_prompts.py  # Templates cho trích xuất
│   ├── utils/                     # Tiện ích chung
│   │   └── __init__.py
│   └── __init__.py
├── config/                        # Cấu hình
│   └── settings.py               # Cài đặt và API keys
├── tests/                        # Test cases
├── docs/                         # Tài liệu
├── main.py                       # Entry point chính
├── requirements.txt              # Dependencies
└── README.md                     # Tài liệu này
```

## 🚀 Tính Năng Chính

### 1. **Trích Xuất Từ Khóa Bằng Sáng Chế** (`src/core/`)
- Hệ thống 3 giai đoạn: Concept Matrix → Seed Keywords → Enhanced Keywords
- Tích hợp LangGraph để quản lý workflow
- Human-in-the-loop validation
- Tự động sinh từ đồng nghĩa và mở rộng từ khóa

### 2. **Tích Hợp API** (`src/api/`)
- **IPC Classification**: Phân loại bằng sáng chế theo tiêu chuẩn quốc tế
- **Brave Search**: Tìm kiếm bằng sáng chế liên quan
- **Tavily Search**: Thu thập thông tin bổ sung

### 3. **Thu Thập Dữ Liệu** (`src/crawling/`)
- Crawler cho Google Patents
- Trích xuất title, abstract, claims, description
- Xử lý lỗi và retry logic

### 4. **Đánh Giá Tương Tự** (`src/evaluation/`)
- Sentence Transformers cho cosine similarity
- BGE Reranker cho precision cao
- LLM-based evaluation với Qwen3

### 5. **Quản Lý Prompt** (`src/prompts/`)
- Template hóa tất cả prompts
- Structured output với Pydantic
- Multilingual support

## 📦 Cài Đặt

```bash
# Clone repository
git clone https://github.com/chienthan2vn/priorart_p.git
cd priorart_p

# Cài đặt dependencies
pip install -r requirements.txt

# Thiết lập biến môi trường (tùy chọn)
cp .env.example .env
# Chỉnh sửa .env với API keys của bạn
```

## 🎮 Sử Dụng

### 🎭 Demo Mode - Khuyến Nghị cho Test/Demo

```bash
# Không cần LLM infrastructure - sử dụng mock responses
python run_demo.py
# hoặc
streamlit run streamlit_demo_app.py --server.port=8502
```

**🎯 Tính năng Demo Mode:**
- **🎭 Mock LLM**: Không cần Ollama hay API keys
- **📝 Complete Workflow**: Toàn bộ quy trình extraction
- **🎯 Real Interaction**: Human evaluation thật (approve/reject/edit)
- **📊 Full Results**: Kết quả hoàn chỉnh với export
- **⚡ Instant Setup**: Chạy ngay không cần cấu hình

👉 **Xem hướng dẫn demo**: [DEMO_README.md](DEMO_README.md)

### 🌐 Giao Diện Web (Real LLM)

```bash
# Cách 1: Sử dụng script khởi chạy
python run_streamlit.py

# Cách 2: Chạy trực tiếp Streamlit  
streamlit run streamlit_app.py
```

**🎯 Tính năng Web Interface:**
- **🤖 Real AI**: LLM thật với Ollama
- **📝 Input Processing**: Nhập mô tả ý tưởng bằng sáng chế
- **🎯 Interactive Evaluation**: Ba lựa chọn tương tác:
  - ✅ **Approve**: Chấp nhận từ khóa và tiếp tục
  - ❌ **Reject**: Từ chối và khởi động lại với feedback  
  - ✏️ **Edit**: Chỉnh sửa từ khóa thủ công
- **📊 Visual Results**: Hiển thị kết quả theo tabs
- **💾 Export Options**: Xuất JSON/CSV cho phân tích thêm

👉 **Xem hướng dẫn chi tiết**: [STREAMLIT_README.md](STREAMLIT_README.md)

### 🚀 Unified Launcher

```bash
# Chọn interface từ menu tương tác
python run_patent_agent.py
```

### 💻 Giao Diện Dòng Lệnh

```bash
python main.py
```

### Sử Dụng Từng Module

```python
from src.core.extractor import CoreConceptExtractor

# Khởi tạo extractor
extractor = CoreConceptExtractor(model_name="qwen3:4b")

# Chạy extraction
results = extractor.extract_keywords(your_patent_text)
```

### Sử Dụng API Modules

```python
# IPC Classification
from src.api.ipc_classifier import get_ipc_predictions
predictions = get_ipc_predictions("your patent summary")

# Patent Crawling
from src.crawling.patent_crawler import PatentCrawler
crawler = PatentCrawler()
patent_info = crawler.extract_patent_info("patent_url")

# Similarity Evaluation
from src.evaluation.similarity_evaluator import PatentSimilarityEvaluator
evaluator = PatentSimilarityEvaluator()
scores = evaluator.evaluate_similarity(text1, text2)
```

## ⚙️ Cấu Hình

Tất cả cấu hình được quản lý trong `config/settings.py`:

```python
from config.settings import settings

# Truy cập cài đặt
print(settings.DEFAULT_MODEL_NAME)
print(settings.MAX_SEARCH_RESULTS)

# Kiểm tra API keys
validation = settings.validate_api_keys()
```

## 🧪 Testing

```bash
# Chạy tests (khi có)
python -m pytest tests/

# Test từng module
python -c "from src.core.extractor import CoreConceptExtractor; print('Core module OK')"
python -c "from src.api.ipc_classifier import get_ipc_predictions; print('API module OK')"
```

## 📋 Workflow Chi Tiết

### 1. **Concept Extraction Phase**
- Input: Ý tưởng bằng sáng chế dạng text
- Output: Concept Matrix (Problem/Purpose, Object/System, Environment/Field)

### 2. **Keyword Generation Phase** 
- Input: Concept Matrix
- Output: Seed Keywords cho từng category

### 3. **Human Evaluation Phase**
- User có thể: Approve, Reject, hoặc Edit keywords
- Interactive interface trong terminal

### 4. **Enhancement Phase**
- Tự động mở rộng keywords bằng web search
- Sinh synonyms và related terms

### 5. **Query Generation Phase**
- Tạo Boolean search queries cho patent databases
- Tích hợp CPC codes từ IPC classification

### 6. **Patent Search & Evaluation Phase**
- Tìm kiếm patents liên quan trên Google Patents
- Đánh giá relevance scores

## 🔧 Dependencies Chính

- **LangChain**: Framework cho LLM applications
- **LangGraph**: Workflow orchestration
- **Pydantic**: Data validation và serialization
- **Sentence-Transformers**: Semantic similarity
- **Transformers**: Hugging Face models
- **BeautifulSoup**: Web scraping
- **Requests**: HTTP client

## 🤝 Đóng Góp

1. Fork repository
2. Tạo feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Tạo Pull Request

## 📜 License

Dự án này được phân phối dưới license MIT. Xem file `LICENSE` để biết thêm chi tiết.

## 🆘 Hỗ Trợ

- **Issues**: [GitHub Issues](https://github.com/chienthan2vn/priorart_p/issues)
- **Documentation**: `/docs` directory
- **Email**: [contact info if available]

---

**Lưu ý**: Kiến trúc mới này đã được tối ưu hóa để dễ bảo trì, mở rộng và testing. Mỗi module có trách nhiệm rõ ràng và có thể sử dụng độc lập.
