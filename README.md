# ComicAI
Shamekh Al Suwi - AI M.Sc - Practical Work - JKU Linz

## Prerequisite    
-python 3.12.9
-docker

```bash

# Install the venv requirements

python -m venv comicai # Remove-Item -Recurse -Force comicai
comicai\Scripts\activate # deactivate
pip install -r requirements.txt

# Install Open WebUI

docker run -d `
    -p 3000:8080  `
    --add-host=host.docker.internal:host-gateway  `
    -v open-webui:/app/backend/data  `
    --name openwebui  `
    --restart always  `
    -e ENABLE_ADMIN_CHAT_ACCESS=false  `
    -e ENABLE_ADMIN_EXPORT=false  `
    -e ENV="dev"  `
    -e ENABLE_IMAGE_GENERATION=true `
    -e COMFYUI_BASE_URL=http://host.docker.internal:8188 `
    -e WEBUI_NAME="Comic AI" ghcr.io/open-webui/open-webui:main

# Install Ollama to handle Huggingface models 

docker run -d  `
    --gpus=all  `
    --name ollama `
    --restart always  `
    -v ollama:/root/.ollama  `
    -p 11434:11434  `
    --name ollama ollama/ollama


# ComfyUI NVDIA users

# there is a reason why comfyui original code has no docker container my advise would be to save time and avoid knowing why
#pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu124

git clone https://github.com/comfyanonymous/ComfyUI
cd comfyui
pip install -r requirements.txt
python main.py --listen
cd ..

# Searxng

mkdir searxng
cd searxng
docker pull searxng/searxng

docker run --rm `
    -d -p 32768:8080 `
    -v "${PWD}/searxng:/etc/searxng" `
    -e "BASE_URL=http://localhost:32768/" `
    -e "INSTANCE_NAME=searxng" `
    --name searxng searxng/searxng

cd ..

# Text-To-Speech service by edge browser

docker run -d -p 5050:5050  -e API_KEY=usability10  travisvn/openai-edge-tts:latest

```
