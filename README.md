# VIPNet RAG Pipeline

**English** | [Русский](#russian)

---

## English

### Overview
This repository contains a complete **Retrieval-Augmented Generation (RAG)** pipeline for technical documentation of **VIPNet Coordinator HW** (Russian cybersecurity product). The pipeline:

- Parses PDF documents, extracting text, tables, CLI commands while preserving structure (headings, sections).
- Splits content into overlapping chunks.
- Builds **dense embeddings** (multilingual-e5-base) with FAISS index and **BM25** sparse index.
- Implements **hybrid search** (weighted sum of dense + BM25) achieving **Hit@K=0.95** on a custom benchmark.
- Optionally integrates **Saiga Llama3 8B** (Russian LLM) for answer generation (requires GPU).

All heavy data (embeddings, FAISS index) are cached to disk; only the first run requires computation.

### ✨ Features
-  PDF parser with font-based heading detection (H1/H2/H3), table extraction, CLI command recognition.
-  Hybrid search: dense (FAISS) + sparse (BM25) with tunable weights (α=0.6 optimal).
-  Evaluation metrics: Hit@K, MRR.
-  Optional LLM integration (Saiga) for RAG answer generation (8-bit quantized or GGUF).
-  Caching of chunks, embeddings, FAISS index for fast restart.
-  Clean, modular code suitable for experimentation and production.

###  Repository Structure
- `vipnet-rag-pipeline.ipynb` – main Jupyter notebook.
- `requirements.txt` – Python dependencies.
- `.gitignore` – standard Python + project-specific ignores.
- `doc_01_chunks.json` – example chunks (JSON format) for quick start (included).
- `rag_index/` – directory where cached data (embeddings, FAISS) are stored (excluded from git).

###  Getting Started

#### Prerequisites
- Google Colab (recommended) or local Jupyter with Python 3.10+.
- GPU recommended for LLM generation (Colab T4 works).

#### Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/vipnet-rag-pipeline.git
   cd vipnet-rag-pipeline
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
For GGUF version of Saiga, additionally install llama-cpp-python (see notebook).

Usage in Colab
Open vipnet-rag-pipeline.ipynb in Colab.

Mount your Google Drive (if you have your own PDFs) or use the provided example chunks.

Run the cells sequentially:

Cells 1–2: install dependencies, configure paths.

Cells 3–4: define helper functions and parser (not needed if using cached chunks).

Cell 5: load example chunks (doc_01_chunks.json).

Cell 6: optional analysis of chunks.

Cell 8: load or create embeddings and FAISS index (first run creates them; subsequent runs load from disk).

Cell 9: define search functions (hybrid weighted search is the main one).

Cell 10: demo hybrid search.

Cell 11: (optional) run benchmark (if you have benchmark_20.json).

Cell 12: (optional) load Saiga LLM and run full RAG pipeline (requires GPU).

Results
On a custom benchmark of 20 questions based on the VIPNet Coordinator HW documentation:

Metric	Value
Hit@K	0.95
MRR	0.767
Configuration
Paths and parameters are set in Cell 2 (edit DOC_01_PATH, INDEX_DIR, etc.).

Chunk size, overlap, font thresholds can be adjusted there.
Hybrid search weight alpha can be tuned in hybrid_weighted_search() (default 0.6).

LLM Integration
Two options are provided:
Transformers (8-bit quantized) – uses bitsandbytes, slower but simpler.
GGUF (llama-cpp-python) – faster on GPU, recommended.
Set LOAD_LLM = True in Cell 12 and choose your preferred method (commented code).
Note: LLM requires GPU; without it, generation will be extremely slow.

License
This project is licensed under the MIT License – see the LICENSE file for details.

Acknowledgements
IlyaGusev/saiga_llama3_8b for the Russian LLM.
intfloat/multilingual-e5-base for embeddings.
FAISS for similarity search.
pdfplumber and PyMuPDF for PDF processing.

# VIPNet RAG Pipeline

**English** | [Русский](#russian)

---

## <a name="russian"></a> Русский

### Обзор
Репозиторий содержит полный пайплайн **Retrieval-Augmented Generation (RAG)** для технической документации **VIPNet Coordinator HW** (российский продукт кибербезопасности). Пайплайн:

- Парсит PDF-документы, извлекая текст, таблицы, CLI-команды с сохранением структуры (заголовки, разделы).
- Разбивает содержимое на чанки с перекрытием.
- Строит **плотные эмбеддинги** (multilingual-e5-base) с индексом FAISS и **разреженный индекс BM25**.
- Реализует **гибридный поиск** (взвешенная сумма dense + BM25) с метрикой **Hit@K=0.95** на собственном бенчмарке.
- Опционально интегрирует **Saiga Llama3 8B** (русскоязычная LLM) для генерации ответов (требуется GPU).

Все тяжёлые данные (эмбеддинги, индекс FAISS) кэшируются на диск; только первый запуск требует вычислений.

### ✨ Возможности
-  Парсер PDF с определением заголовков по шрифтам (H1/H2/H3), извлечением таблиц, распознаванием CLI-команд.
-  Гибридный поиск: dense (FAISS) + sparse (BM25) с настраиваемым весом (оптимально α=0.6).
-  Метрики оценки: Hit@K, MRR.
-  Опциональная LLM (Saiga) для генерации ответов (8-битное квантование или GGUF).
-  Кэширование чанков, эмбеддингов, FAISS-индекса для быстрого перезапуска.
-  Чистый, модульный код для экспериментов и продакшена.

###  Структура репозитория
- `vipnet-rag-pipeline.ipynb` – основной ноутбук.
- `requirements.txt` – зависимости Python.
- `.gitignore` – стандартные игнорирования для Python + специфичные для проекта.
- `doc_01_chunks.json` – пример чанков (формат JSON) для быстрого старта (опционально).
- `rag_index/` – папка для кэшированных данных (исключена из git).

###  Начало работы

#### Требования
- Google Colab (рекомендуется) или локальный Jupyter с Python 3.10+.
- Для генерации LLM желателен GPU (в Colab подойдёт T4).

#### Установка
1. Клонируйте репозиторий:
   ```bash
   git clone https://github.com/yourusername/vipnet-rag-pipeline.git
   cd vipnet-rag-pipeline
   ```
2. Установите зависимости:
    ```bash
   pip install -r requirements.txt
    ```
Для GGUF-версии Saiga дополнительно установите llama-cpp-python (см. ноутбук).

Использование в Colab
Откройте vipnet-rag-pipeline.ipynb в Colab.

Подключите Google Drive (если используете свои PDF) или работайте с примером чанков.

Выполните ячейки последовательно:

Ячейки 1–2: установка зависимостей, настройка путей.

Ячейки 3–4: вспомогательные функции и парсер (необязательно, если используете готовые чанки).

Ячейка 5: загрузка примеров чанков (doc_01_chunks.json).

Ячейка 6: опциональный анализ чанков.

Ячейка 8: загрузка или создание эмбеддингов и FAISS-индекса (при первом запуске создаются, затем подгружаются с диска).

Ячейка 9: определение функций поиска (основная – hybrid_weighted_search).

Ячейка 10: демонстрация гибридного поиска.

Ячейка 11: (опционально) запуск бенчмарка (если есть benchmark_20.json).

Ячейка 12: (опционально) загрузка Saiga LLM и запуск полного RAG-пайплайна (требуется GPU).

Результаты
На тестовом наборе из 20 вопросов по документации VIPNet Coordinator HW:

Метрика	Значение
Hit@K	0.95
MRR	0.767

Настройка
Пути и параметры задаются в Ячейке 2 (можно изменить DOC_01_PATH, INDEX_DIR и др.).
Размер чанков, перекрытие, пороги шрифтов настраиваются там же.
Вес гибридного поиска alpha регулируется в функции hybrid_weighted_search() (по умолчанию 0.6).

Интеграция LLM
Предусмотрено два варианта:

Transformers (8-битное квантование) – использует bitsandbytes, медленнее, но проще.
GGUF (llama-cpp-python) – быстрее на GPU, рекомендуется.
Установите LOAD_LLM = True в Ячейке 12 и выберите нужный вариант (код закомментирован).
Важно: LLM требует GPU; без него генерация будет крайне медленной.

Лицензия
Проект распространяется под лицензией MIT – подробности в файле LICENSE.

 Благодарности
IlyaGusev/saiga_llama3_8b за русскоязычную LLM.
intfloat/multilingual-e5-base за эмбеддинги.
FAISS за поиск по сходству.
pdfplumber и PyMuPDF за обработку PDF.
