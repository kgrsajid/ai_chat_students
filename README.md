# 🎓 AI Chat Backend

FastAPI-based Python backend for AI-powered chat with educational content for Kazakhstan school students.

## ✨ Features

### AI Chat with Context Memory
- Full conversation history is maintained per session
- AI remembers previous messages and can reference them
- Natural, contextual responses

### OpenAI Integration
- Powered by GPT-4o-mini for fast, accurate responses
- Fallback to direct AI responses when vector DB is empty
- Streaming support for real-time responses

### Vector Search (Optional)
- Pinecone integration for educational content search
- **Now optional** - chat works fully with OpenAI only
- Upload study materials for contextual answers

### Multilingual Support
- 🇬🇧 English
- 🇷🇺 Русский
- 🇰🇿 Қазақша

## 🚀 Quick Start

### Prerequisites
- Python 3.10+
- OpenAI API key

### Installation

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Configuration

Create `.env` file:

```env
OPENAI_API_KEY=sk-your-openai-api-key
# PINECONE_API_KEY=pc-your-pinecone-api-key  # Optional
```

**Note:** Pinecone is optional. Without it, the chat uses OpenAI directly for all responses.

### Running

```bash
# API Server
python api_server.py

# Access docs at http://localhost:8000/docs
```

## 🌐 API Endpoints

### Chat
- `POST /chat` - Send message (with session context)
- `POST /chat/new` - Create new session & send first message
- `WebSocket /ws/{session_id}` - Real-time streaming chat

### Sessions
- `GET /history/{user_id}` - Get chat history
- `DELETE /session/{user_id}` - Clear session

### Materials
- `POST /upload` - Upload study materials
- `GET /subjects` - List available subjects
- `GET /stats` - Knowledge base statistics

### Other
- `GET /health` - Health check
- `GET /summary` - Generate topic summary

## 📖 Usage Examples

### Send a Message

```bash
curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "student123",
    "message": "Explain derivatives",
    "language": "ru"
  }'
```

### Create New Session

```bash
curl -X POST http://localhost:8000/chat/new \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "student123",
    "message": "What is photosynthesis?",
    "language": "en"
  }'
```

### Get Chat History

```bash
curl http://localhost:8000/history/student123
```

### WebSocket Chat (JavaScript)

```javascript
const ws = new WebSocket(
  `ws://localhost:8000/ws/session123?user_id=student123&language=ru`
);

ws.onopen = () => {
  ws.send("Explain the water cycle");
};

ws.onmessage = (event) => {
  console.log(event.data);  // Streaming response
};
```

## 📁 Project Structure

```
chatbackend/
├── api_server.py          # FastAPI server
├── school_ai_platform.py   # AI platform core
├── school_topics.json      # Subject knowledge base
├── requirements.txt        # Python dependencies
├── .env                    # Environment variables
└── README.md
```

## 🎓 Educational Features

### Subject Areas
- Mathematics (Algebra, Geometry, Calculus)
- Physics (Mechanics, Optics, Electricity)
- Chemistry (Organic, Inorganic, Physical)
- Biology (Cell biology, Genetics, Ecology)
- History (World, Kazakhstan)
- Geography
- Languages (Kazakh, Russian, English)

### Response Modes
- **Direct AI**: General questions answered by OpenAI
- **Contextual**: Uses uploaded materials when available
- **Streaming**: Real-time token-by-token responses

## 🔧 Configuration Options

| Variable | Required | Description |
|----------|----------|-------------|
| `OPENAI_API_KEY` | Yes | Your OpenAI API key |
| `PINECONE_API_KEY` | No | For vector search (chat works without it) |
| `PINECONE_INDEX` | No | Pinecone index name (default: school-topics) |

## 🐳 Docker

```bash
docker compose up -d
```

## 📝 License

MIT
