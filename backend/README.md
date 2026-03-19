# Naadify Music Generator Backend

A serverless music generation service powered by Modal that uses ACE-Step for audio synthesis, Qwen2 for LLM capabilities, and Stable Diffusion for image generation.

## Prerequisites

- Python 3.10+
- [Modal CLI](https://modal.com/docs/guide/cli) installed and authenticated
- Git
- CUDA-capable GPU (for local testing)

## Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd naadify-music-generator/backend
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Setup Modal**
   ```bash
   modal setup
   ```

## Configuration

### Required Modal Secrets

Create a secret named `music-gen-secret` in Modal:
```bash
modal secret create music-gen-secret
```

### Required Modal Volumes

- `ace-step-models` - Stores ACE-Step checkpoint files
- `qwen-hf-cache` - Caches Hugging Face models

These are created automatically on first run.

## Running the Service

### Deploy to Modal
```bash
modal run main.py
```

This will:
- Build the Docker image with all dependencies
- Deploy the MusicGenServer class
- Start the FastAPI endpoint
- Generate an audio file locally

### Endpoints

- **POST** `/generate` - Generate music based on the hardcoded prompt

**Response:** JSON with base64-encoded audio data
```json
{
  "audio_data": "<base64-encoded-wav>"
}
```

## Project Structure

- `main.py` - Main application with Modal deployment configuration
- `prompts.py` - Prompt management module (currently empty)
- `requirements.txt` - Python dependencies with pinned versions

## Key Dependencies

- **transformers** - Qwen2 LLM for text processing
- **diffusers** - Stable Diffusion for image generation
- **torch** - PyTorch deep learning framework
- **pydantic** - Data validation
- **requests** - HTTP client for API calls
- **modal** - Serverless platform (installed separately)

## Models Used

- **ACE-Step** - Audio generation model
- **Qwen2-7B-Instruct** - Large language model for instruction following
- **Stable Diffusion XL Turbo** - Fast image generation

## Development Notes

- The service runs on A10 GPU for optimal performance
- Models are cached in Modal volumes to avoid repeated downloads
- Audio is base64-encoded for API transmission
- Default output format: WAV, 180 seconds, 120 BPM

## Project Status

### ✅ Completed
- [x] ACE-Step model integration for audio synthesis
- [x] Qwen2-7B LLM support for text understanding
- [x] Stable Diffusion XL Turbo for image generation
- [x] Modal serverless deployment configuration
- [x] Base64 audio encoding/decoding for API transmission
- [x] Docker image with FFmpeg and dependencies
- [x] Volume management for model caching
- [x] FastAPI endpoint setup
- [x] Version compatibility fixes (transformers 4.45.0, diffusers 0.33.0)

### 🚧 In Progress
- [ ] Streamline prompts.py for dynamic prompt generation
- [ ] Add comprehensive error handling and logging
- [ ] Implement health check endpoints
- [ ] Performance optimization and response time improvements

### 📋 TODO
- [ ] Add frontend UI for music generation
- [ ] Create interactive API documentation (Swagger)
- [ ] Implement webhook support for async generation
- [ ] Add batch processing capabilities
- [ ] Create monitoring dashboard
- [ ] Add unit and integration tests
- [ ] Documentation for extending the service

## Troubleshooting

**ImportError with `Qwen3ForCausalLM`**
- Ensure `requirements.txt` has pinned versions for `transformers` and `diffusers`
- Rebuild the Modal image: `modal run main.py --build`

**Volume mount errors**
- Volume paths must be absolute (starting with `/`)
- Verify volume names match those referenced in `main.py`

**GPU Memory issues**
- A10 GPU provides 24GB VRAM, sufficient for all three models
- If encountering OOM errors, check volume allocations

**Audio save errors**
- Use `soundfile` backend for WAV encoding (pure Python)
- Ensure FFmpeg is installed via `apt_install("ffmpeg")`
- Rebuild the Modal image if dependencies change

## License

See LICENSE file in parent directory.
