# Mazo Job Description Skills Extractor & Interview Question Generator

A powerful Streamlit-based web application that automatically extracts technical skills from job descriptions and generates relevant interview questions using Google's Gemini AI.

![Mazo Logo](https://mazobeam.com/wp-content/uploads/2023/12/mazoid-1.png)

## 🚀 Features

- **📄 Document Upload**: Upload job descriptions in PDF or Word (DOCX) format
- **🔍 Intelligent Skill Extraction**: Combines regex pattern matching with AI-powered extraction for comprehensive skill identification
- **✍️ Manual Skills Input**: Option to manually enter skills if you prefer
- **🤖 AI-Powered Question Generation**: Generates context-aware interview questions using Google Gemini AI
- **⚙️ Customizable Parameters**:
  - Experience level (1-50 years)
  - Question complexity (Basic, Intermediate, Advanced)
  - Number of questions (1-100)
- **📥 Word Export**: Download generated questions as a formatted Word document
- **🎨 Modern UI**: Clean, user-friendly interface built with Streamlit

## 📋 Prerequisites

- Python 3.8 or higher
- Google Gemini API key ([Get one here](https://aistudio.google.com/welcome))
- Internet connection (for API calls)

## 🛠️ Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd Mazo_JD_Question_Answer-main
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Set up environment variables**
   
   Create a `.env` file in the project root:
   ```env
   GEMINI_API_KEY=your_google_gemini_api_key_here
   ```

4. **Run the application**
   ```bash
   streamlit run app.py
   ```

5. **Open your browser**
   
   The application will automatically open at `http://localhost:8501`

## 📖 Usage

### Method 1: Upload Job Description

1. Select **"Upload Job Description"** option
2. Upload a PDF or DOCX file containing the job description
3. Wait for skills to be extracted automatically
4. Review the extracted skills
5. Configure:
   - Experience level (years)
   - Question complexity (Basic/Intermediate/Advanced)
   - Number of questions
6. Click **"Generate Questions"**
7. Review the generated questions and answers
8. Click **"Download as Word"** to export

### Method 2: Manual Skills Input

1. Select **"Manually Input Skills"** option
2. Enter skills separated by commas (e.g., `Python, SQL, Data Analysis`)
3. Configure experience level, complexity, and question count
4. Click **"Generate Questions"**
5. Download the results as a Word document

## 🎯 Question Complexity Levels

- **Basic**: Pre-screening questions focusing on fundamental terminology and concepts. Ideal for quickly identifying candidates who lack essential knowledge.
- **Intermediate**: Main interview assessment questions suitable for evaluating qualified candidates.
- **Advanced**: Specialist-level questions for experienced professionals and deep technical assessments.

## 📦 Dependencies

- `streamlit` - Web application framework
- `google-generativeai` - Google Gemini API client
- `python-docx` - Word document processing
- `PyPDF2` - PDF text extraction
- `python-dotenv` - Environment variable management

See `requirements.txt` for complete list.

## 🔧 Configuration

### API Key Setup

1. Visit [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a new API key
3. Add it to your `.env` file:
   ```
   GEMINI_API_KEY=your_api_key_here
   ```

### Supported File Formats

- **PDF**: `.pdf` files (text-based PDFs, not scanned images)
- **Word Documents**: `.docx` files

## 🏗️ Project Structure

```
Mazo_JD_Question_Answer-main/
├── app.py                 # Main application file
├── requirements.txt       # Python dependencies
├── .env                  # Environment variables (create this)
├── README.md             # This file
├── TECHNICAL_DOCUMENTATION.md  # Detailed technical documentation
└── LICENSE               # License file
```

## 🐛 Troubleshooting

### API Key Issues
- Ensure `.env` file exists in the project root
- Verify `GEMINI_API_KEY` variable name is correct
- Check that your API key is valid and has sufficient quota

### File Upload Issues
- Ensure PDFs are text-based (not scanned images)
- Check file size (very large files may take longer)
- Verify file format is supported (.pdf or .docx)

### No Skills Extracted
- Try manual skills input as an alternative
- Check if the job description contains technical terms
- Verify API connection is working

### Export Issues
- Ensure sufficient disk space
- Check browser download permissions
- Try generating fewer questions if document is too large

## 🔒 Security Notes

- **Never commit your `.env` file** to version control
- Keep your API key secure and private
- The application sends job description data to Google's API
- Consider data privacy policies when processing sensitive information

## 🚀 Deployment

### Streamlit Cloud

1. Push your code to GitHub
2. Go to [Streamlit Cloud](https://streamlit.io/cloud)
3. Connect your repository
4. Add `GEMINI_API_KEY` in the secrets section
5. Deploy!

### Local Server

```bash
streamlit run app.py --server.port 8501 --server.address 0.0.0.0
```

## 📝 Example

**Input (Job Description)**:
```
We are looking for a Data Engineer with experience in Python, SQL, 
AWS, and ETL processes. Knowledge of Spark and data warehousing 
is required.
```

**Extracted Skills**:
```
Python, SQL, AWS, ETL, Spark, Data Warehousing, Data Engineering
```

**Generated Questions** (Intermediate, 10 years experience):
- Question 1: Explain the difference between ETL and ELT processes...
- Question 2: How would you optimize a Spark job for large datasets...
- (and more...)

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

See the [LICENSE](LICENSE) file for details.

## 📞 Support

For detailed technical documentation, see [TECHNICAL_DOCUMENTATION.md](TECHNICAL_DOCUMENTATION.md).

For issues or questions:
- Check the troubleshooting section above
- Review the technical documentation
- Verify your API key and configuration

## 🙏 Acknowledgments

- Built with [Streamlit](https://streamlit.io/)
- Powered by [Google Gemini AI](https://ai.google.dev/)
- Logo and branding by [MazoBeam](https://mazobeam.com/)

---

**Made with ❤️ for efficient hiring processes**

