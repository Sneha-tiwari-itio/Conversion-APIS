# Document Conversion Backend

A Node.js backend service for converting between DOC/DOCX and PDF formats using popular npm libraries.

## Features

- ✅ Convert DOC/DOCX files to PDF
- ✅ Convert PDF files to DOCX
- ✅ File upload with validation
- ✅ Automatic file cleanup
- ✅ RESTful API endpoints
- ✅ CORS enabled
- ✅ Security headers with Helmet
- ✅ Error handling and logging

## Prerequisites

- Node.js (v14 or higher)
- npm or yarn

## Installation

1. **Clone or download the project files**

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Start the server:**
   ```bash
   # Development mode (with auto-restart)
   npm run dev
   
   # Production mode
   npm start
   ```

The server will start on `http://localhost:3000` by default.

## API Endpoints

### Health Check
```
GET /health
```
Returns server status and basic information.

### Get Supported Formats
```
GET /api/conversion/formats
```
Returns information about supported file formats and conversion options.

### Convert DOC/DOCX to PDF
```
POST /api/conversion/doc-to-pdf
```
**Form Data:**
- `document`: The DOC or DOCX file to convert

**Response:**
```json
{
  "success": true,
  "message": "Document converted successfully",
  "originalFile": "document.docx",
  "outputFile": "document-1234567890.pdf",
  "downloadUrl": "/outputs/document-1234567890.pdf",
  "fileSize": 12345
}
```

### Convert PDF to DOCX
```
POST /api/conversion/pdf-to-doc
```
**Form Data:**
- `document`: The PDF file to convert

**Response:**
```json
{
  "success": true,
  "message": "Document converted successfully",
  "originalFile": "document.pdf",
  "outputFile": "document-1234567890.docx",
  "downloadUrl": "/outputs/document-1234567890.docx",
  "fileSize": 12345
}
```

## Usage Examples

### Using cURL

**Convert DOC to PDF:**
```bash
curl -X POST \
  http://localhost:3000/api/conversion/doc-to-pdf \
  -F "document=@/path/to/your/document.docx"
```

**Convert PDF to DOCX:**
```bash
curl -X POST \
  http://localhost:3000/api/conversion/pdf-to-doc \
  -F "document=@/path/to/your/document.pdf"
```

### Using JavaScript/Fetch

```javascript
// Convert DOC to PDF
async function convertDocToPdf(file) {
  const formData = new FormData();
  formData.append('document', file);
  
  const response = await fetch('http://localhost:3000/api/conversion/doc-to-pdf', {
    method: 'POST',
    body: formData
  });
  
  const result = await response.json();
  console.log(result);
  
  // Download the converted file
  if (result.success) {
    window.open(`http://localhost:3000${result.downloadUrl}`, '_blank');
  }
}

// Convert PDF to DOCX
async function convertPdfToDoc(file) {
  const formData = new FormData();
  formData.append('document', file);
  
  const response = await fetch('http://localhost:3000/api/conversion/pdf-to-doc', {
    method: 'POST',
    body: formData
  });
  
  const result = await response.json();
  console.log(result);
  
  // Download the converted file
  if (result.success) {
    window.open(`http://localhost:3000${result.downloadUrl}`, '_blank');
  }
}
```

### Using Python

```python
import requests

# Convert DOC to PDF
def convert_doc_to_pdf(file_path):
    url = "http://localhost:3000/api/conversion/doc-to-pdf"
    
    with open(file_path, 'rb') as file:
        files = {'document': file}
        response = requests.post(url, files=files)
    
    result = response.json()
    print(result)
    
    # Download the converted file
    if result['success']:
        download_url = f"http://localhost:3000{result['downloadUrl']}"
        print(f"Download URL: {download_url}")

# Convert PDF to DOCX
def convert_pdf_to_doc(file_path):
    url = "http://localhost:3000/api/conversion/pdf-to-doc"
    
    with open(file_path, 'rb') as file:
        files = {'document': file}
        response = requests.post(url, files=files)
    
    result = response.json()
    print(result)
    
    # Download the converted file
    if result['success']:
        download_url = f"http://localhost:3000{result['downloadUrl']}"
        print(f"Download URL: {download_url}")
```

## File Structure

```
Backend/
├── server.js                 # Main server file
├── package.json             # Dependencies and scripts
├── README.md               # This file
├── routes/
│   └── conversion.js       # API routes for conversion
├── services/
│   ├── conversionService.js           # Basic conversion service
│   └── enhancedConversionService.js   # Enhanced conversion with better formatting
├── uploads/                # Temporary upload directory (auto-created)
└── outputs/                # Converted files directory (auto-created)
```

## Configuration

### Environment Variables

You can set the following environment variables:

- `PORT`: Server port (default: 3000)
- `NODE_ENV`: Environment mode (development/production)

### File Size Limits

- Maximum file size: 50MB
- Supported formats: `.doc`, `.docx`, `.pdf`

## Libraries Used

- **Express**: Web framework
- **Multer**: File upload handling
- **Mammoth**: DOC/DOCX to HTML conversion with styling
- **Puppeteer**: HTML to PDF conversion with full rendering
- **PDF-parse**: PDF text extraction and parsing
- **Docx**: DOCX file creation with proper formatting
- **CORS**: Cross-origin resource sharing
- **Helmet**: Security headers
- **fs-extra**: Enhanced file system operations

## Limitations and Notes

### Current Limitations

1. **PDF to DOCX**: Now uses `pdf-parse` for actual text extraction, but complex formatting may not be perfectly preserved.

2. **Images**: Image handling in PDFs is limited to text extraction only.

3. **Complex Layouts**: Very complex document layouts may not be perfectly maintained during conversion.

### Production Considerations

1. **File Storage**: Consider using cloud storage (AWS S3, Google Cloud Storage) instead of local storage.

2. **Queue System**: For high traffic, implement a job queue (Redis, Bull) for background processing.

3. **Authentication**: Add authentication and authorization for production use.

4. **Rate Limiting**: Implement rate limiting to prevent abuse.

5. **Monitoring**: Add logging and monitoring (Winston, Sentry).

## Troubleshooting

### Common Issues

1. **Port already in use**: Change the PORT environment variable
2. **File upload fails**: Check file size and format
3. **Conversion errors**: Ensure the input file is not corrupted

### Error Responses

The API returns appropriate HTTP status codes:
- `200`: Success
- `400`: Bad request (invalid file, missing file)
- `500`: Internal server error

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is licensed under the ISC License. 