# Technical Documentation

## Mazo Job Description Skills Extractor & Interview Question Generator

### Table of Contents
1. [Overview](#overview)
2. [Architecture](#architecture)
3. [System Requirements](#system-requirements)
4. [Dependencies](#dependencies)
5. [API Integration](#api-integration)
6. [Core Components](#core-components)
7. [Data Flow](#data-flow)
8. [Configuration](#configuration)
9. [Error Handling](#error-handling)
10. [Performance Considerations](#performance-considerations)
11. [Security Considerations](#security-considerations)
12. [Testing](#testing)
13. [Deployment](#deployment)
14. [Troubleshooting](#troubleshooting)

---

## Overview

The Mazo Job Description Skills Extractor & Interview Question Generator is a Streamlit-based web application that automates the process of extracting technical skills from job descriptions and generating relevant interview questions. The application leverages Google's Gemini AI API for intelligent skill extraction and question generation.

### Key Features
- **Dual Input Methods**: Upload job description files (PDF/DOCX) or manually input skills
- **Hybrid Skill Extraction**: Combines regex pattern matching with AI-powered extraction
- **Intelligent Question Generation**: Generates context-aware interview questions based on skills and experience level
- **Export Functionality**: Exports generated questions to formatted Word documents
- **Customizable Parameters**: Adjustable experience level, complexity, and question count

---

## Architecture

### Technology Stack
- **Frontend Framework**: Streamlit (Python-based web framework)
- **AI/ML Service**: Google Gemini 1.5 Flash API
- **Document Processing**: 
  - `python-docx` for Word document handling
  - `PyPDF2` for PDF text extraction
- **Environment Management**: `python-dotenv` for configuration

### Application Structure
```
app.py
├── Configuration & Setup
├── Skill Extraction Module
│   ├── Regex-based matching
│   └── Gemini API integration
├── Document Processing Module
│   ├── PDF text extraction
│   └── DOCX text extraction
├── Question Generation Module
│   └── Gemini API integration
├── Export Module
│   └── Word document generation
└── Streamlit UI
```

---

## System Requirements

### Software Requirements
- **Python**: 3.8 or higher
- **Operating System**: Windows, Linux, or macOS
- **Web Browser**: Modern browser with JavaScript enabled (Chrome, Firefox, Edge, Safari)

### Hardware Requirements
- **RAM**: Minimum 4GB (8GB recommended)
- **Storage**: 100MB free space
- **Network**: Internet connection for API calls

---

## Dependencies

### Core Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `streamlit` | Latest | Web application framework |
| `google-generativeai` | Latest | Google Gemini API client |
| `python-docx` | Latest | Word document creation and parsing |
| `PyPDF2` | Latest | PDF text extraction |
| `python-dotenv` | Latest | Environment variable management |

### Additional Dependencies

| Package | Purpose |
|---------|---------|
| `pandas` | Data manipulation (if needed for future features) |
| `openpyxl` | Excel file support (if needed for future features) |
| `spacy` | NLP processing (if needed for future features) |
| `fpdf` | PDF generation (if needed for future features) |

### Installation
```bash
pip install -r requirements.txt
```

---

## API Integration

### Google Gemini API

#### Configuration
The application uses Google Gemini 1.5 Flash model for both skill extraction and question generation.

#### API Key Setup
1. Obtain API key from [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a `.env` file in the project root
3. Add the following line:
   ```
   GEMINI_API_KEY=your_api_key_here
   ```

#### API Endpoints Used
- **Model**: `gemini-1.5-flash`
- **Generation Config**:
  - Temperature: 1.0
  - Top-p: 0.95
  - Top-k: 40
  - Max output tokens: 8192
  - Response MIME type: text/plain

#### Rate Limiting
- The application makes sequential API calls
- Consider implementing rate limiting for production use
- Monitor API usage through Google Cloud Console

---

## Core Components

### 1. Skill Extraction Module

#### `match_skill(text, skills_list)`
- **Purpose**: Extracts skills using regex pattern matching
- **Input**: Text string, predefined skills list
- **Output**: Set of matched skills
- **Algorithm**: Word boundary matching with case-insensitive search
- **Time Complexity**: O(n × m) where n = number of skills, m = text length

#### `gemini_extract_skills(text)`
- **Purpose**: Extracts skills using AI-powered analysis
- **Input**: Job description text
- **Output**: Set of extracted skills
- **Process**:
  1. Sends text to Gemini API with extraction prompt
  2. Parses comma-separated response
  3. Returns unique skills set
- **Error Handling**: Returns empty set on failure

#### `extract_skills(job_description)`
- **Purpose**: Combines regex and AI extraction methods
- **Input**: Complete job description text
- **Output**: Combined list of unique skills
- **Strategy**: Union of regex and Gemini results for comprehensive coverage

### 2. Document Processing Module

#### `extract_text_from_docx(docx_file)`
- **Purpose**: Extracts text from Word documents
- **Input**: DOCX file object
- **Output**: Plain text string
- **Process**: Iterates through document paragraphs
- **Error Handling**: Displays error message via Streamlit

#### `extract_text_from_pdf(pdf_file)`
- **Purpose**: Extracts text from PDF documents
- **Input**: PDF file object
- **Output**: Plain text string
- **Process**: Iterates through PDF pages
- **Limitations**: May not extract text from scanned images
- **Error Handling**: Displays error message via Streamlit

### 3. Question Generation Module

#### `generate_interview_questions(skills, experience_level, complexity, num_questions)`
- **Purpose**: Generates interview questions using AI
- **Input Parameters**:
  - `skills`: List of technical skills
  - `experience_level`: Years of experience (1-50)
  - `complexity`: "Basic", "Intermediate", or "Advanced"
  - `num_questions`: Number of questions to generate (1-100)
- **Output**: Formatted text with questions and answers
- **Complexity Levels**:
  - **Basic**: Pre-screening questions focusing on fundamental concepts
  - **Intermediate**: Main interview assessment questions
  - **Advanced**: Specialist-level questions for experienced professionals
- **Error Handling**: Returns empty string on failure

### 4. Export Module

#### `export_to_word(data, job_name)`
- **Purpose**: Creates formatted Word document
- **Input**:
  - `data`: List of Q&A dictionaries
  - `job_name`: Document title prefix
- **Output**: BytesIO object containing Word document
- **Formatting**:
  - Questions: Center-aligned
  - Answers: Left-aligned
  - Font size: 12pt
  - Title: "{job_name} - Interview Questions and Answers"
- **Error Handling**: Returns None on failure

### 5. Predefined Skills List

The application includes a predefined list of common technical skills for regex matching:

```python
PREDEFINED_SKILLS = [
    "Python", "Java", "SQL", "Machine Learning", "Data Analysis",
    "Data Engineering", "AWS", "Azure", "Docker", "Kubernetes",
    "ETL", "Big Data", "Hadoop", "Spark", "Tableau", "Power BI",
    "AWS Glue", "PySpark", "Aurora DB", "Dynamo DB", "Redshift",
    "Data Warehousing", "CI/CD", "Stone branch", "Scheduling Tool"
]
```

**Note**: This list can be extended based on domain requirements.

---

## Data Flow

### Upload Job Description Flow
```
1. User uploads PDF/DOCX file
   ↓
2. File type detection
   ↓
3. Text extraction (extract_text_from_pdf/docx)
   ↓
4. Skill extraction (extract_skills)
   ├── Regex matching (match_skill)
   └── AI extraction (gemini_extract_skills)
   ↓
5. Skills displayed to user
   ↓
6. User configures parameters (experience, complexity, count)
   ↓
7. Question generation (generate_interview_questions)
   ↓
8. Results displayed and export option provided
```

### Manual Skills Input Flow
```
1. User enters skills manually
   ↓
2. Skills parsed and validated
   ↓
3. User configures parameters
   ↓
4. Question generation
   ↓
5. Results displayed and export option provided
```

---

## Configuration

### Environment Variables

Create a `.env` file in the project root:

```env
GEMINI_API_KEY=your_google_gemini_api_key_here
```

### Application Configuration

#### Streamlit Configuration
- **Page Title**: "Mazo"
- **Page Icon**: 📄
- **Default Values**:
  - Experience Level: 10 years
  - Question Count: 10
  - Complexity: User-selected (Basic/Intermediate/Advanced)

#### Gemini API Configuration
- **Model**: gemini-1.5-flash
- **Temperature**: 1.0 (high creativity)
- **Max Tokens**: 8192
- **Response Format**: Plain text

### Customization Options

#### Adding New Skills
Edit the `PREDEFINED_SKILLS` list in `app.py`:
```python
PREDEFINED_SKILLS.append("New Skill Name")
```

#### Adjusting API Parameters
Modify the `generation_config` dictionary in:
- `gemini_extract_skills()` function
- `generate_interview_questions()` function

---

## Error Handling

### Error Types and Handling

1. **File Upload Errors**
   - Unsupported file type: Error message displayed
   - Empty file: Warning message displayed
   - Extraction failure: Error logged and displayed

2. **API Errors**
   - Network failures: Error message with exception details
   - API key issues: Check environment variable configuration
   - Rate limiting: Implement retry logic (future enhancement)

3. **Document Processing Errors**
   - Corrupted files: Error message displayed
   - Unreadable PDF: Error logged
   - DOCX parsing errors: Exception caught and displayed

4. **Export Errors**
   - File generation failure: Error message displayed
   - Memory issues: Handle large document generation

### Error Messages
All errors are displayed using Streamlit's error components:
- `st.error()`: For critical errors
- `st.warning()`: For warnings
- `st.info()`: For informational messages

---

## Performance Considerations

### Optimization Strategies

1. **API Call Optimization**
   - Sequential API calls (consider async for production)
   - Caching extracted skills (future enhancement)
   - Batch processing for multiple documents (future enhancement)

2. **Document Processing**
   - PDF extraction can be slow for large files
   - Consider implementing progress bars
   - Stream processing for large documents

3. **Memory Management**
   - Word document generation uses BytesIO for memory efficiency
   - Large PDFs may consume significant memory

### Performance Metrics
- **Skill Extraction**: ~2-5 seconds (depends on API response time)
- **Question Generation**: ~5-15 seconds (depends on question count and API)
- **Document Export**: <1 second for typical documents

---

## Security Considerations

### API Key Security
- **Never commit `.env` file to version control**
- Use environment variables in production
- Rotate API keys regularly
- Monitor API usage for unauthorized access

### Input Validation
- File type validation before processing
- File size limits (consider adding)
- Sanitize user inputs before API calls

### Data Privacy
- Job descriptions may contain sensitive information
- API calls send data to external service (Google)
- Consider data retention policies
- Implement session-based data cleanup

### Recommendations
1. Add file size limits
2. Implement input sanitization
3. Add rate limiting for API calls
4. Consider local AI models for sensitive data
5. Implement user authentication (future enhancement)

---

## Testing

### Unit Testing Recommendations

```python
# Example test structure
def test_match_skill():
    text = "Experience with Python and SQL required"
    skills = match_skill(text, PREDEFINED_SKILLS)
    assert "Python" in skills
    assert "SQL" in skills

def test_extract_text_from_pdf():
    # Test PDF extraction with sample file
    pass

def test_generate_interview_questions():
    # Mock API response and test question generation
    pass
```

### Integration Testing
- Test complete workflow with sample job descriptions
- Test error handling scenarios
- Test export functionality

### Manual Testing Checklist
- [ ] Upload PDF job description
- [ ] Upload DOCX job description
- [ ] Manual skills input
- [ ] Different complexity levels
- [ ] Various experience levels
- [ ] Different question counts
- [ ] Word document export
- [ ] Error scenarios (invalid files, API failures)

---

## Deployment

### Local Development
```bash
# Install dependencies
pip install -r requirements.txt

# Set up environment variables
echo "GEMINI_API_KEY=your_key" > .env

# Run application
streamlit run app.py
```

### Production Deployment

#### Streamlit Cloud
1. Push code to GitHub repository
2. Connect repository to Streamlit Cloud
3. Add environment variables in Streamlit Cloud settings
4. Deploy application

#### Docker Deployment
```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

#### Environment Variables in Production
- Use secure secret management (AWS Secrets Manager, Azure Key Vault, etc.)
- Never hardcode API keys
- Use different API keys for different environments

---

## Troubleshooting

### Common Issues

#### 1. API Key Not Found
**Error**: `Failed to extract skills using Gemini API`
**Solution**: 
- Verify `.env` file exists in project root
- Check `GEMINI_API_KEY` variable name
- Ensure API key is valid

#### 2. PDF Text Extraction Fails
**Error**: `Error extracting text from PDF`
**Solution**:
- Verify PDF is not password-protected
- Check if PDF contains text (not scanned images)
- Try converting PDF to DOCX first

#### 3. No Skills Extracted
**Error**: `No skills found in the job description`
**Solution**:
- Check if job description contains technical terms
- Verify API key is working
- Try manual skills input as workaround

#### 4. Export Fails
**Error**: `Error exporting to Word`
**Solution**:
- Check available disk space
- Verify write permissions
- Check if generated content is valid

#### 5. Application Won't Start
**Error**: Import errors or missing dependencies
**Solution**:
- Run `pip install -r requirements.txt`
- Verify Python version (3.8+)
- Check virtual environment activation

### Debug Mode
Enable Streamlit debug mode:
```bash
streamlit run app.py --logger.level=debug
```

### Logging
Consider adding structured logging:
```python
import logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)
```

---

## Future Enhancements

### Recommended Improvements
1. **Caching**: Cache extracted skills and generated questions
2. **Async Processing**: Use async API calls for better performance
3. **Multiple Export Formats**: Support PDF, Excel, JSON exports
4. **Question Templates**: Pre-defined question templates by role
5. **Skill Database**: Maintain a comprehensive skills database
6. **User Authentication**: Add user accounts and history
7. **Batch Processing**: Process multiple job descriptions
8. **Analytics**: Track usage and popular skills
9. **Custom Prompts**: Allow users to customize AI prompts
10. **Multi-language Support**: Support multiple languages

---

## Version History

### Current Version: 1.0.0
- Initial release
- Basic skill extraction (regex + AI)
- Question generation with complexity levels
- Word document export
- PDF and DOCX support

---

## Support and Contact

For technical support or questions:
- Review this documentation
- Check error messages in the application
- Verify API key and configuration
- Review Google Gemini API documentation

---

## License

See LICENSE file for details.

