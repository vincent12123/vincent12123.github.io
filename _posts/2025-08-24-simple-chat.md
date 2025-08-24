---
title: Simple Chat Bot
date: 2025-08-24
categories: [Chat, Flask]
tags: [GeminiAi]
pin: true
---
# Tutorial Lengkap: Membuat Chatbot dengan Flask dan Gemini AI

## Daftar Isi
1. [Pengenalan](#pengenalan)
2. [Prasyarat](#prasyarat)
3. [Setup Environment](#setup-environment)
4. [Struktur Project](#struktur-project)
5. [Backend Development (Flask)](#backend-development-flask)
6. [Frontend Development](#frontend-development)
7. [Integrasi Gemini AI](#integrasi-gemini-ai)
8. [Testing dan Debugging](#testing-dan-debugging)
9. [Deployment](#deployment)
10. [Troubleshooting](#troubleshooting)

---

## Pengenalan

Dalam tutorial ini, kita akan membangun chatbot sederhana namun powerful menggunakan:
- **Flask** sebagai web framework backend
- **Google Gemini AI** sebagai AI engine
- **HTML/CSS/JavaScript** untuk frontend
- **Bootstrap** untuk styling yang responsive

### Apa yang akan kita buat?
- Chatbot dengan interface yang modern dan responsive
- Real-time conversation dengan AI
- History percakapan
- Error handling yang baik
- Mobile-friendly interface

---

## Prasyarat

### Software yang Dibutuhkan:
- Python 3.8 atau lebih baru
- Text editor (VS Code, Sublime, dll)
- Browser modern
- Git (opsional)

### Pengetahuan yang Diperlukan:
- Dasar Python
- HTML/CSS dasar
- JavaScript dasar (akan dijelaskan)
- Konsep dasar web development

---

## Setup Environment

### 1. Membuat Virtual Environment

```bash
# Navigasi ke direktori project
cd /home/sukardi/coding/simplechatbot

# Buat virtual environment
python3 -m venv .venv

# Aktivasi virtual environment
source .venv/bin/activate  # Linux/Mac
# atau
# .venv\Scripts\activate  # Windows
```

### 2. Install Dependencies

Buat file `requirements.txt`:
```txt
flask==2.3.3
google-generativeai==0.3.2
python-dotenv==1.0.0
gunicorn==21.2.0
Werkzeug==2.3.7
```

Install semua dependencies:
```bash
pip install -r requirements.txt
```

### 3. Setup Environment Variables

Buat file `.env`:
```bash
cp .env.example .env
```

Edit `.env` dan masukkan API key Gemini:
```env
GEMINI_API_KEY=your_actual_gemini_api_key_here
FLASK_ENV=development
FLASK_DEBUG=True
SECRET_KEY=your_secret_key_here
```

### 4. Mendapatkan Gemini API Key

1. Kunjungi [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Login dengan akun Google
3. Klik "Create API Key"
4. Copy API key dan paste ke file `.env`

---

## Struktur Project

```
simplechatbot/
├── app.py                 # Flask application utama
├── requirements.txt       # Dependencies Python
├── .env                  # Environment variables (jangan commit!)
├── .env.example          # Template environment variables
├── .gitignore           # File yang diabaikan Git
├── README.md            # Dokumentasi project
├── templates/
│   └── index.html       # Template HTML utama
└── static/
    ├── style.css        # CSS styling
    └── script.js        # JavaScript functionality
```

---

## Backend Development (Flask)

### 1. Setup Flask Application (`app.py`)

```python
import os
import google.generativeai as genai
from flask import Flask, render_template, request, jsonify
from dotenv import load_dotenv

# Load environment variables dari file .env
load_dotenv()

# Inisialisasi Flask app
app = Flask(__name__)
```

**Penjelasan:**
- `load_dotenv()`: Memuat variabel environment dari file `.env`
- `Flask(__name__)`: Membuat instance Flask app

### 2. Konfigurasi Gemini AI

```python
# Configure Gemini dengan API key dari environment variable
genai.configure(api_key=os.getenv('GEMINI_API_KEY'))

# Initialize Gemini model - menggunakan model terbaru
model = genai.GenerativeModel('gemini-2.5-flash')

# Store conversation history dalam memory
conversation_history = []
```

**Penjelasan:**
- `genai.configure()`: Setup API key untuk Gemini
- `GenerativeModel()`: Inisialisasi model AI
- `conversation_history`: List untuk menyimpan riwayat percakapan

### 3. Route untuk Halaman Utama

```python
@app.route('/')
def index():
    """Render halaman utama chatbot"""
    return render_template('index.html')
```

**Penjelasan:**
- Route ini akan menampilkan file `templates/index.html` ketika user mengakses URL root

### 4. API Endpoint untuk Chat

```python
@app.route('/chat', methods=['POST'])
def chat():
    """Handle pesan chat dari user"""
    try:
        # Ambil pesan dari request JSON
        user_message = request.json.get('message', '').strip()
        
        # Validasi pesan tidak kosong
        if not user_message:
            return jsonify({'error': 'Message cannot be empty'}), 400
        
        # Tambah pesan user ke history
        conversation_history.append(f"User: {user_message}")
        
        # Buat context dari 10 pesan terakhir untuk memberikan konteks ke AI
        context = "\n".join(conversation_history[-10:])
        
        # Generate response menggunakan Gemini
        response = model.generate_content(f"{context}\nUser: {user_message}")
        bot_response = response.text
        
        # Tambah response bot ke history
        conversation_history.append(f"Assistant: {bot_response}")
        
        # Return response dalam format JSON
        return jsonify({
            'response': bot_response,
            'status': 'success'
        })
        
    except Exception as e:
        # Handle error dan return error message
        return jsonify({
            'error': f'Error generating response: {str(e)}',
            'status': 'error'
        }), 500
```

**Penjelasan Detail:**
- `request.json.get('message')`: Mengambil pesan dari POST request
- `conversation_history[-10:]`: Mengambil 10 pesan terakhir untuk konteks
- `model.generate_content()`: Memanggil Gemini AI untuk generate response
- Error handling dengan try-except untuk menangani error

### 5. API Endpoint Tambahan

```python
@app.route('/clear', methods=['POST'])
def clear_history():
    """Clear riwayat percakapan"""
    global conversation_history
    conversation_history = []
    return jsonify({'status': 'success', 'message': 'History cleared'})

@app.route('/health')
def health_check():
    """Health check endpoint untuk monitoring"""
    return jsonify({'status': 'healthy'})
```

### 6. Main Function

```python
if __name__ == '__main__':
    # Check apakah API key sudah diset
    if not os.getenv('GEMINI_API_KEY'):
        print("Warning: GEMINI_API_KEY not found in environment variables")
        print("Please set your Gemini API key in the .env file")
    
    # Jalankan Flask app
    app.run(debug=True, host='0.0.0.0', port=5000)
```

---

## Frontend Development

### 1. HTML Template (`templates/index.html`)

#### Head Section:
```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Chatbot - Gemini AI</title>
    <!-- Bootstrap CSS untuk styling -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Font Awesome untuk icons -->
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <!-- Custom CSS -->
    <link href="{{ url_for('static', filename='style.css') }}" rel="stylesheet">
</head>
```

#### Header Chat:
```html
<div class="chat-header">
    <div class="d-flex justify-content-between align-items-center">
        <div class="d-flex align-items-center">
            <i class="fas fa-robot me-2 text-primary"></i>
            <h4 class="mb-0">Simple Chatbot</h4>
            <small class="text-muted ms-2">Powered by Gemini AI</small>
        </div>
        <button class="btn btn-outline-danger btn-sm" onclick="clearChat()">
            <i class="fas fa-trash me-1"></i> Clear Chat
        </button>
    </div>
</div>
```

#### Chat Messages Area:
```html
<div id="chatMessages" class="chat-messages">
    <div class="message bot-message">
        <div class="message-avatar">
            <i class="fas fa-robot"></i>
        </div>
        <div class="message-content">
            <div class="message-text">
                Halo! Saya adalah chatbot yang didukung oleh Gemini AI. Apa yang bisa saya bantu hari ini?
            </div>
            <div class="message-time" id="initial-time"></div>
        </div>
    </div>
</div>
```

#### Input Area:
```html
<div class="chat-input">
    <div class="input-group">
        <input type="text" 
               id="messageInput" 
               class="form-control" 
               placeholder="Ketik pesan Anda disini..." 
               autocomplete="off">
        <button class="btn btn-primary" 
                id="sendButton" 
                onclick="sendMessage()">
            <i class="fas fa-paper-plane"></i>
        </button>
    </div>
</div>
```

### 2. CSS Styling (`static/style.css`)

#### CSS Variables:
```css
:root {
    --primary-color: #007bff;
    --secondary-color: #6c757d;
    --success-color: #28a745;
    --danger-color: #dc3545;
    --border-radius: 15px;
    --shadow: 0 2px 10px rgba(0,0,0,0.1);
}
```

#### Body Styling:
```css
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    margin: 0;
    padding: 0;
    height: 100vh;
    overflow: hidden;
}
```

#### Chat Container:
```css
.chat-container {
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    border-radius: 0 0 var(--border-radius) var(--border-radius);
    height: calc(100vh - 140px);
    display: flex;
    flex-direction: column;
    box-shadow: var(--shadow);
}
```

#### Message Styling:
```css
.message {
    display: flex;
    margin-bottom: 20px;
    animation: messageSlide 0.3s ease-out;
}

@keyframes messageSlide {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}
```

### 3. JavaScript Functionality (`static/script.js`)

#### Global Variables:
```javascript
let isLoading = false;
const loadingModal = new bootstrap.Modal(document.getElementById('loadingModal'), {
    keyboard: false,
    backdrop: 'static'
});
```

#### Initialize App:
```javascript
document.addEventListener('DOMContentLoaded', function() {
    // Set initial timestamp
    document.getElementById('initial-time').textContent = getCurrentTime();
    
    // Focus pada input
    document.getElementById('messageInput').focus();
    
    // Event listener untuk Enter key
    document.getElementById('messageInput').addEventListener('keypress', function(e) {
        if (e.key === 'Enter' && !e.shiftKey) {
            e.preventDefault();
            sendMessage();
        }
    });
});
```

#### Send Message Function:
```javascript
async function sendMessage() {
    const messageInput = document.getElementById('messageInput');
    const message = messageInput.value.trim();
    
    if (!message || isLoading) {
        return;
    }
    
    // Disable input dan button
    setLoadingState(true);
    
    // Tambah pesan user ke chat
    addMessage(message, 'user');
    
    // Clear input
    messageInput.value = '';
    
    // Show loading modal
    loadingModal.show();
    
    try {
        // Kirim pesan ke backend
        const response = await fetch('/chat', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({ message: message })
        });
        
        const data = await response.json();
        
        if (response.ok && data.status === 'success') {
            // Tambah response bot ke chat
            addMessage(data.response, 'bot');
        } else {
            // Show error message
            addMessage(`Maaf, terjadi kesalahan: ${data.error}`, 'bot', 'error');
        }
        
    } catch (error) {
        console.error('Error:', error);
        addMessage('Maaf, terjadi kesalahan koneksi. Silakan coba lagi.', 'bot', 'error');
    } finally {
        // Hide loading modal dan restore input
        loadingModal.hide();
        setLoadingState(false);
        messageInput.focus();
    }
}
```

#### Add Message Function:
```javascript
function addMessage(message, sender, type = 'normal') {
    const chatMessages = document.getElementById('chatMessages');
    const messageDiv = document.createElement('div');
    
    const isUser = sender === 'user';
    const messageClass = isUser ? 'user-message' : 'bot-message';
    const avatarIcon = isUser ? 'fas fa-user' : 'fas fa-robot';
    
    messageDiv.className = `message ${messageClass}`;
    messageDiv.innerHTML = `
        <div class="message-avatar">
            <i class="${avatarIcon}"></i>
        </div>
        <div class="message-content">
            <div class="message-text ${type === 'error' ? 'error-message' : ''}">
                ${formatMessage(message)}
            </div>
            <div class="message-time">${getCurrentTime()}</div>
        </div>
    `;
    
    chatMessages.appendChild(messageDiv);
    scrollToBottom();
}
```

---

## Integrasi Gemini AI

### 1. Setup API Key

```python
import google.generativeai as genai

# Konfigurasi API key
genai.configure(api_key=os.getenv('GEMINI_API_KEY'))
```

### 2. Inisialisasi Model

```python
# Gunakan model terbaru Gemini
model = genai.GenerativeModel('gemini-2.5-flash')
```

**Model Gemini yang tersedia:**
- `gemini-pro`: Model text generasi umum
- `gemini-pro-vision`: Model dengan kemampuan vision
- `gemini-2.5-flash`: Model terbaru yang lebih cepat

### 3. Generate Content

```python
# Generate response dengan konteks
response = model.generate_content(f"{context}\nUser: {user_message}")
bot_response = response.text
```

### 4. Handling Context

```python
# Menyimpan riwayat percakapan untuk konteks
conversation_history.append(f"User: {user_message}")
context = "\n".join(conversation_history[-10:])  # 10 pesan terakhir
conversation_history.append(f"Assistant: {bot_response}")
```

---

## Testing dan Debugging

### 1. Menjalankan Aplikasi

```bash
# Pastikan virtual environment aktif
source .venv/bin/activate

# Jalankan aplikasi
python app.py
```

### 2. Testing Manual

1. Buka browser dan akses `http://localhost:5000`
2. Test kirim pesan sederhana: "Halo"
3. Test pesan yang lebih kompleks
4. Test fitur clear chat
5. Test error handling (matikan internet)

### 3. Debug Common Issues

#### API Key Error:
```python
# Tambah debug print
if not os.getenv('GEMINI_API_KEY'):
    print("API key tidak ditemukan!")
    print("Pastikan file .env sudah dibuat dan berisi GEMINI_API_KEY")
```

#### Connection Error:
```javascript
// Di JavaScript, tambah error handling
catch (error) {
    console.error('Error details:', error);
    addMessage('Koneksi bermasalah. Cek console untuk detail.', 'bot', 'error');
}
```

### 4. Logging

```python
import logging

# Setup logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

@app.route('/chat', methods=['POST'])
def chat():
    try:
        user_message = request.json.get('message', '').strip()
        logger.info(f"Received message: {user_message}")
        
        # ... rest of code
        
        logger.info(f"Generated response: {bot_response}")
        
    except Exception as e:
        logger.error(f"Error in chat endpoint: {str(e)}")
```

---

## Deployment

### 1. Production Setup

#### Gunicorn Configuration (`gunicorn.conf.py`):
```python
bind = "0.0.0.0:5000"
workers = 4
timeout = 120
keepalive = 60
max_requests = 1000
max_requests_jitter = 100
```

#### Run dengan Gunicorn:
```bash
gunicorn -c gunicorn.conf.py app:app
```

### 2. Deploy ke Heroku

#### Procfile:
```
web: gunicorn app:app
```

#### Commands:
```bash
# Login ke Heroku
heroku login

# Buat app
heroku create your-chatbot-app

# Set environment variables
heroku config:set GEMINI_API_KEY=your_api_key

# Deploy
git add .
git commit -m "Initial commit"
git push heroku main
```

### 3. Deploy ke VPS

#### Setup Script (`deploy.sh`):
```bash
#!/bin/bash

# Update system
sudo apt update && sudo apt upgrade -y

# Install Python
sudo apt install python3 python3-pip python3-venv -y

# Install Nginx
sudo apt install nginx -y

# Clone project
git clone your-repo-url /var/www/chatbot

# Setup virtual environment
cd /var/www/chatbot
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Setup systemd service
sudo cp chatbot.service /etc/systemd/system/
sudo systemctl enable chatbot
sudo systemctl start chatbot

# Setup Nginx
sudo cp nginx.conf /etc/nginx/sites-available/chatbot
sudo ln -s /etc/nginx/sites-available/chatbot /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

### 4. Environment Variables Production

```bash
# .env untuk production
GEMINI_API_KEY=your_production_api_key
FLASK_ENV=production
FLASK_DEBUG=False
SECRET_KEY=your_super_secret_production_key
```

---

## Troubleshooting

### 1. Common Errors

#### "GEMINI_API_KEY not found"
**Solusi:**
```bash
# Check apakah file .env ada
ls -la .env

# Check isi file .env
cat .env

# Pastikan tidak ada spasi di sekitar API key
# Salah: GEMINI_API_KEY = your_key
# Benar: GEMINI_API_KEY=your_key
```

#### "Module not found"
**Solusi:**
```bash
# Check virtual environment aktif
which python

# Install ulang dependencies
pip install -r requirements.txt

# Check versi Python
python --version
```

#### "Port already in use"
**Solusi:**
```bash
# Cari process yang menggunakan port 5000
lsof -i :5000

# Kill process
kill -9 PID_NUMBER

# Atau gunakan port lain
python app.py --port 5001
```

### 2. Performance Issues

#### Slow Response Time:
```python
# Tambah timeout ke Gemini API
import asyncio

async def generate_with_timeout(prompt, timeout=30):
    try:
        return await asyncio.wait_for(
            model.generate_content_async(prompt), 
            timeout=timeout
        )
    except asyncio.TimeoutError:
        return "Maaf, response time terlalu lama. Coba lagi."
```

#### Memory Issues:
```python
# Limit conversation history
MAX_HISTORY = 20

def add_to_history(message):
    conversation_history.append(message)
    if len(conversation_history) > MAX_HISTORY:
        conversation_history.pop(0)
```

### 3. Security Issues

#### API Key Security:
```python
# Jangan hardcode API key
# Salah:
# genai.configure(api_key="AIzaSyC...")

# Benar:
api_key = os.getenv('GEMINI_API_KEY')
if not api_key:
    raise ValueError("GEMINI_API_KEY must be set")
genai.configure(api_key=api_key)
```

#### Input Validation:
```python
def validate_message(message):
    if not message or not message.strip():
        return False, "Message cannot be empty"
    
    if len(message) > 1000:
        return False, "Message too long"
    
    # Check for malicious content
    dangerous_patterns = ['<script>', 'javascript:', 'eval(']
    if any(pattern in message.lower() for pattern in dangerous_patterns):
        return False, "Invalid content detected"
    
    return True, None

@app.route('/chat', methods=['POST'])
def chat():
    user_message = request.json.get('message', '').strip()
    
    is_valid, error = validate_message(user_message)
    if not is_valid:
        return jsonify({'error': error}), 400
```

---

## Tips dan Best Practices

### 1. Code Organization

```python
# Gunakan Blueprint untuk organize routes
from flask import Blueprint

chatbot_bp = Blueprint('chatbot', __name__)

@chatbot_bp.route('/chat', methods=['POST'])
def chat():
    # chat logic here
    pass

# Register blueprint
app.register_blueprint(chatbot_bp)
```

### 2. Configuration Management

```python
# config.py
import os

class Config:
    SECRET_KEY = os.getenv('SECRET_KEY', 'dev-secret-key')
    GEMINI_API_KEY = os.getenv('GEMINI_API_KEY')
    
class DevelopmentConfig(Config):
    DEBUG = True
    
class ProductionConfig(Config):
    DEBUG = False

# app.py
from config import DevelopmentConfig

app.config.from_object(DevelopmentConfig)
```

### 3. Error Handling

```python
@app.errorhandler(404)
def not_found(error):
    return jsonify({'error': 'Endpoint not found'}), 404

@app.errorhandler(500)
def internal_error(error):
    return jsonify({'error': 'Internal server error'}), 500
```

### 4. Rate Limiting

```python
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address

limiter = Limiter(
    app,
    key_func=get_remote_address,
    default_limits=["100 per hour"]
)

@app.route('/chat', methods=['POST'])
@limiter.limit("10 per minute")
def chat():
    # chat logic
    pass
```

---

## Pengembangan Selanjutnya

### 1. Fitur Tambahan

- **User Authentication**: Login/logout system
- **Chat History Persistence**: Simpan ke database
- **Multiple Chat Rooms**: Support multiple conversations
- **File Upload**: Upload gambar untuk Gemini Vision
- **Voice Chat**: Speech-to-text integration

### 2. Improvements

- **Caching**: Redis untuk cache response
- **WebSocket**: Real-time communication
- **Mobile App**: React Native atau Flutter
- **Analytics**: Track usage dan performance

### 3. Advanced Features

```python
# Streaming response
@app.route('/chat-stream', methods=['POST'])
def chat_stream():
    def generate():
        for chunk in model.generate_content_stream(prompt):
            yield f"data: {chunk.text}\n\n"
    
    return Response(generate(), mimetype='text/plain')
```

---

## Kesimpulan

Tutorial ini telah membahas secara lengkap cara membuat chatbot menggunakan Flask dan Gemini AI, mulai dari setup environment hingga deployment production. 

### Key Points:
1. **Backend**: Flask dengan endpoint REST API
2. **AI Integration**: Google Gemini AI untuk generate response
3. **Frontend**: HTML/CSS/JS dengan Bootstrap
4. **Security**: Proper API key handling dan input validation
5. **Deployment**: Multiple deployment options

### Next Steps:
1. Experiment dengan prompt engineering
2. Tambah fitur sesuai kebutuhan
3. Optimize performance untuk production
4. Monitor dan maintain aplikasi

Selamat coding! 🚀

---

*Tutorial ini dibuat berdasarkan implementasi aktual chatbot Flask-Gemini. Untuk pertanyaan atau issue, silakan buat ticket di repository project.*
