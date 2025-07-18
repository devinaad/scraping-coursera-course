# scraping-coursera-course
# 📚 Coursera Course Scraper & Data Preprocessing

A Python-based web scraper that extracts course information from Coursera and preprocesses the data for analysis and machine learning applications.

## 📊 Data Overview

### Data Source
- **Platform**: Coursera (https://www.coursera.org)
- **Data Collection Method**: Web scraping using BeautifulSoup
- **Search Query**: User-defined (e.g., "machine learning", "data science")

### Dataset Features
The scraped dataset contains **588 rows × 5 columns** with the following information:

| Column | Description | Example |
|--------|-------------|---------|
| **Title** | Course title | "Machine Learning with Python" |
| **Organization** | Course provider | "IBM", "DeepLearning.AI" |
| **Skills** | Skills taught in the course | "Supervised Learning, Python Programming" |
| **Metadata** | Course difficulty, type, and duration | "Beginner · Course · 1 - 3 Months" |

### Data Statistics
- **Total Courses**: 588 courses
- **Pages Scraped**: Up to 49 pages (12 courses per page)
- **Data Format**: CSV file
- **File Size**: Varies based on query results

## 🔧 Scraping Process

### Tools & Libraries Used
```python
import requests          # HTTP requests to fetch web pages
from bs4 import BeautifulSoup  # HTML parsing and extraction
from urllib.parse import quote_plus  # URL encoding
import pandas as pd      # Data manipulation and storage
```

### Scraping Flow

1. **Input Processing**
   - User enters search query (e.g., "machine learning")
   - Query is URL-encoded using `quote_plus()`

2. **URL Generation**
   ```python
   url = f"https://www.coursera.org/search?query={encoded_query}&page={page_number}"
   ```

3. **Data Extraction**
   - **Course Titles**: `<h3 class="cds-CommonCard-title">`
   - **Organizations**: `<p class="cds-ProductCard-partnerNames">`
   - **Course Difficulty**: `<p class="css-vac8rf">` in metadata sections
   - **Skills**: `<p class="css-vac8rf">` in body content sections

4. **Data Validation**
   - Ensures exactly 12 courses per page
   - Handles missing data with `None` values
   - Maintains data consistency across all columns

5. **Data Storage**
   - Combines all scraped data into pandas DataFrame
   - Exports to CSV file: `coursera_course_dataset.csv`

### Key Scraping Function
```python
def auto_Scrapper_Class(user_query, html_tag, course_case, tag_class, div_class=None):
    """
    Main scraping function that extracts course information
    - Handles pagination automatically
    - Processes HTML elements based on specified tags and classes
    - Maintains data integrity across multiple pages
    """
```

## 🧼 Text Preprocessing

### Preprocessing Pipeline
The raw text data undergoes several cleaning steps to prepare it for analysis:

### Tools Used
```python
import re                    # Regular expressions for text cleaning
from nltk.corpus import stopwords  # English stopwords removal
from nltk.stem import PorterStemmer, WordNetLemmatizer  # Text normalization
```

### Preprocessing Steps

1. **Lowercasing**: Convert all text to lowercase
2. **Punctuation Removal**: Remove special characters and symbols
3. **Stopword Removal**: Remove common English words ("the", "and", etc.)
4. **Stemming**: Reduce words to their root form
5. **Lemmatization**: Convert words to their dictionary base form

### Before & After Examples

#### Course Title Processing
```
Before: "Machine Learning with Python"
After:  "machin learn python"
```

#### Skills Processing
```
Before: "Unsupervised Learning, Supervised Learning, Machine Learning Algorithms"
After:  "unsupervis learn supervis learn machin learn algorithm"
```

#### Organization Processing
```
Before: "DeepLearning.AI"
After:  "deeplearningai"
```

### Preprocessing Function
```python
def preprocessing(text: str) -> str:
    text = lowering(text)
    text = remove_punctuation_and_symbol(text)
    text = stopword_removal(text)
    text = stemming(text)
    text = lemmatization(text)
    return text
```

### Output Files
- **Original Dataset**: `coursera_course_dataset.csv`
- **Processed Dataset**: `processed_coursera_course_dataset.csv`

## 🤔 Refleksi & Pembelajaran

### Kendala yang Dihadapi

1. **Website Structure Changes**
   - **Masalah**: CSS class names di Coursera bisa berubah sewaktu-waktu
   - **Solusi**: Menggunakan class names yang lebih stabil dan melakukan testing berkala

2. **Rate Limiting**
   - **Masalah**: Coursera membatasi jumlah request per detik
   - **Solusi**: Tidak menggunakan delay, tapi memantau response untuk menghindari blocked requests

3. **Data Inconsistency**
   - **Masalah**: Beberapa halaman memiliki kurang dari 12 courses
   - **Solusi**: Implementasi validasi data dengan mengisi `None` untuk data yang missing

4. **HTML Parsing Complexity**
   - **Masalah**: Structure HTML yang kompleks dengan nested elements
   - **Solusi**: Menggunakan parameter `div_class` untuk targeting yang lebih spesifik

### Solusi yang Diterapkan

1. **Modular Function Design**
   - Membuat fungsi `auto_Scrapper_Class()` yang fleksibel
   - Dapat digunakan untuk berbagai jenis elemen HTML

2. **Data Validation**
   - Memastikan konsistensi jumlah data di setiap halaman
   - Handling missing values dengan proper null values

3. **Robust Text Preprocessing**
   - Pipeline preprocessing yang comprehensive
   - Normalisasi text untuk analysis yang lebih akurat

4. **Error Handling**
   - Validasi response dari setiap request
   - Graceful handling ketika data tidak lengkap

### Pelajaran yang Didapat

1. **Web Scraping Best Practices**
   - Pentingnya memahami struktur HTML target
   - Perlu antisipasi perubahan website structure

2. **Data Quality**
   - Cleaning data sama pentingnya dengan collecting data
   - Preprocessing yang baik meningkatkan kualitas analysis

3. **Scalability Considerations**
   - Design code yang mudah di-maintain dan di-extend
   - Dokumentasi yang clear untuk future development

## 🚀 Usage Instructions

### Running the Scraper
1. Open `coursera-course-scraping.ipynb`
2. Run all cells in sequence
3. Enter your search query when prompted
4. Wait for scraping to complete

### Running Preprocessing
1. Open `text_preprocessing.ipynb`
2. Ensure the CSV file from scraping exists
3. Run all cells to process the text data

## 📁 Project Structure
```
├── coursera-course-scraping.ipynb     # Main scraping notebook
├── text_preprocessing.ipynb           # Text preprocessing notebook
├── coursera_course_dataset.csv        # Raw scraped data
├── processed_coursera_course_dataset.csv  # Processed data
└── README.md                          # This file
```
---

*Created as part of data science learning project. This tool is for educational purposes only.*