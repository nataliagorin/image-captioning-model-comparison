# 🖼️ Image Captioning Model Comparison

> AI-powered image captioning app comparing NIC (ViT+GPT-2), BLIP, and a custom ResNet152+LSTM+Attention model. Built with Flask & React, includes text-to-speech for generated captions.

---

## ✨ Features

- 🤖 **Three captioning models** — choose between NIC, BLIP, or a custom-trained model
- 🔊 **Text-to-Speech** — captions are automatically converted to audio using gTTS
- ⚡ **Model caching** — models are loaded once and reused to minimize latency
- 🎯 **Beam search** — custom model uses beam search decoding for higher quality captions
- 🌐 **Modern web interface** — React + Vite frontend with a clean UI

---

## 🧠 Models

| Model | Architecture | Pre-trained on | HuggingFace ID |
|-------|-------------|----------------|----------------|
| **NIC** | ViT encoder + GPT-2 decoder | COCO | [`nlpconnect/vit-gpt2-image-captioning`](https://huggingface.co/nlpconnect/vit-gpt2-image-captioning) |
| **BLIP** | BLIP (bootstrapped language-image pretraining) | COCO | [`Salesforce/blip-image-captioning-base`](https://huggingface.co/Salesforce/blip-image-captioning-base) |
| **Custom** | ResNet152 encoder + LSTM decoder + Attention | Flickr30k | trained locally |

### Custom Model Details

The custom model was trained from scratch on the [Flickr30k dataset](https://www.kaggle.com/datasets/hsankesara/flickr-image-dataset) and uses:
- **ResNet152** (pretrained on ImageNet) as a CNN feature extractor
- **Bahdanau-style attention** mechanism over spatial feature maps (7x7x2048)
- **LSTM decoder** with learned initial hidden/cell state from mean encoder features
- **Beam search** (beam size = 3) at inference time

---

## 🗂️ Project Structure

```
image-caption/
├── backend/
│   ├── app.py                      # Flask REST API
│   ├── custom_caption_model.pth    # Trained model weights (Git LFS)
│   └── vocab.json                  # Vocabulary (word2idx / idx2word)
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── ImageUploader.jsx   # Drag-and-drop image input
│   │   │   ├── ModelSelector.jsx   # Model selection UI
│   │   │   ├── CaptionDisplay.jsx  # Generated caption output
│   │   │   ├── TTSControl.jsx      # Audio playback control
│   │   │   └── ErrorBoundary.jsx   # Error handling wrapper
│   │   ├── App.jsx
│   │   └── main.jsx
│   └── package.json
├── train_custom_model.ipynb        # Training notebook (Flickr30k)
└── evaluate_models.ipynb           # Model evaluation & comparison
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.9+
- Node.js 18+
- Git LFS (for downloading the `.pth` model weights)

```bash
git lfs install
git clone https://github.com/nataliagorin/image-captioning-model-comparison.git
```

### Backend Setup

```bash
cd backend

# Create and activate virtual environment
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # macOS/Linux

# Install dependencies
pip install flask flask-cors gtts transformers torch torchvision pillow

# Start the Flask server
python app.py
```

The API will be available at `http://localhost:5000`.

> **Note:** On first run, NIC and BLIP models are automatically downloaded from HuggingFace (~1-2 GB). This may take a few minutes.

### Frontend Setup

```bash
cd frontend

npm install
npm run dev
```

The app will be available at `http://localhost:5173`.

---

## 🔌 API Reference

### `POST /caption`

Generates a caption and audio for an uploaded image.

**Request** — `multipart/form-data`

| Field   | Type   | Values                         |
|---------|--------|--------------------------------|
| `image` | file   | Any image file (JPG, PNG, ...) |
| `model` | string | `nic` \| `blip` \| `custom`   |

**Response** — `application/json`

```json
{
  "caption": "a dog running in the grass",
  "audio": "<base64-encoded MP3>"
}
```

---

## 📓 Notebooks

| Notebook | Description |
|----------|-------------|
| [`train_custom_model.ipynb`](train_custom_model.ipynb) | Full training pipeline for the custom ResNet+LSTM model on Flickr30k |
| [`evaluate_models.ipynb`](evaluate_models.ipynb) | Quantitative and qualitative comparison of all three models |

---

## 🛠️ Tech Stack

**Backend**
- [Flask](https://flask.palletsprojects.com/) — REST API
- [PyTorch](https://pytorch.org/) — model inference
- [HuggingFace Transformers](https://huggingface.co/docs/transformers) — NIC & BLIP models
- [gTTS](https://gtts.readthedocs.io/) — text-to-speech

**Frontend**
- [React](https://react.dev/) + [Vite](https://vitejs.dev/)
- [Axios](https://axios-http.com/) — HTTP client

---

## 📄 License

This project was developed as part of an *Introduction to Big Data* university course.
