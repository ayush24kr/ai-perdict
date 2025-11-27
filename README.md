# Reproductive Health API

This is a unified API that combines both the **Chatbot** and **Menstrual Cycle Predictor** functionalities into a single service.

## Features

✅ **Reproductive Health Chatbot** - AI-powered educational assistant using Groq  
✅ **Menstrual Cycle Predictor** - LSTM/GRU-based cycle prediction  
✅ **Single API** - Run only one service instead of two  
✅ **Unified Health Checks** - Monitor both features from one endpoint  

## Installation

```bash
# Install required dependencies
pip install fastapi uvicorn groq pydantic numpy torch tensorflow
```

## Environment Setup

Set your Groq API key:

```bash
# Windows PowerShell
$env:GROQ_API_KEY="your-api-key-here"

# Windows CMD
set GROQ_API_KEY=your-api-key-here

# Linux/Mac
export GROQ_API_KEY=your-api-key-here
```

## Running the API

```bash
python combined_api.py
```

The API will start on `http://localhost:8000`

## API Endpoints

### 🏠 Root Endpoint
```
GET /
```
Returns API status and available features

### ❤️ Health Check
```
GET /health
```
Check status of both chatbot and cycle predictor

### 💬 Chatbot
```
POST /chat
```

**Request Body:**
```json
{
  "message": "What is a normal menstrual cycle?"
}
```

**Response:**
```json
{
  "response": "Educational response from AI...",
  "safety_triggered": false
}
```

### 📊 Cycle Prediction
```
POST /predict
```

**Request Body:**
```json
{
  "past_cycles": [28, 30, 27, 29, 28, 31, 28, 29, 27, 30],
  "last_period_date": "2025-01-15",
  "framework": "pytorch"
}
```

**Response:**
```json
{
  "predicted_cycle_length": 29,
  "predicted_next_period": "2025-02-13",
  "predicted_next_period_formatted": "Thursday, February 13, 2025",
  "confidence_interval": {
    "predicted_days": 29,
    "min_days": 27,
    "max_days": 31,
    "earliest_date": "2025-02-11",
    "latest_date": "2025-02-15"
  },
  "statistics": {
    "average_cycle_length": 28.7,
    "std_deviation": 1.34,
    "min_cycle": 27,
    "max_cycle": 31,
    "total_cycles_analyzed": 10
  },
  "uncertainty_days": 1.34,
  "framework_used": "pytorch"
}
```

### 🔬 Available Frameworks
```
GET /frameworks
```
Lists available ML frameworks (PyTorch/TensorFlow)

## Testing the API

### Using cURL

**Test Chatbot:**
```bash
curl -X POST "http://localhost:8000/chat" \
  -H "Content-Type: application/json" \
  -d "{\"message\": \"What is ovulation?\"}"
```

**Test Cycle Predictor:**
```bash
curl -X POST "http://localhost:8000/predict" \
  -H "Content-Type: application/json" \
  -d "{\"past_cycles\": [28, 30, 27, 29, 28, 31], \"last_period_date\": \"2025-01-15\", \"framework\": \"pytorch\"}"
```

### Using Python

```python
import requests

# Test Chatbot
response = requests.post(
    "http://localhost:8000/chat",
    json={"message": "What causes period cramps?"}
)
print(response.json())

# Test Cycle Predictor
response = requests.post(
    "http://localhost:8000/predict",
    json={
        "past_cycles": [28, 30, 27, 29, 28, 31, 28, 29],
        "last_period_date": "2025-01-15",
        "framework": "pytorch"
    }
)
print(response.json())
```

## Interactive Documentation

Visit `http://localhost:8000/docs` for interactive Swagger UI documentation where you can test all endpoints directly in your browser.

## Port Configuration

By default, the API runs on port 8000. You can change this using the PORT environment variable:

```bash
# Windows PowerShell
$env:PORT=5000
python combined_api.py

# Linux/Mac
PORT=5000 python combined_api.py
```

## Troubleshooting

### Groq API Key Missing
If you see "❌ Missing" for the Groq API key, make sure you've set the environment variable correctly.

### PyTorch/TensorFlow Not Available
Install the required ML framework:
```bash
pip install torch  # For PyTorch
# OR
pip install tensorflow  # For TensorFlow
```


