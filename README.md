# ComicAI
Shamekh Al Suwi - AI M.Sc - Practical Work - JKU Linz

## Prerequisite    
-python
-docker


```bash
py -3.11 -m venv comicai # Remove-Item -Recurse -Force comicai
comicai\Scripts\activate # deactivate

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


docker run -d  `
    --gpus=all  `
    --name ollama `
    --restart always  `
    -v ollama:/root/.ollama  `
    -p 11434:11434  `
    --name ollama ollama/ollama


# ComfyUI NVDIA users
 git clone https://github.com/comfyanonymous/ComfyUI
cd comfyui

pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu124

pip install -r requirements.txt
#Pull the Docker image (if you haven't already):
docker pull ghcr.io/lecode-official/comfyui-docker:latest
#Save the Docker image to a tar file:
docker save -o comfyui-docker-latest.tar ghcr.io/lecode-official/comfyui-docker:latest
#To load the saved image later, use the docker load command
docker load -i comfyui-docker-latest.tar

docker run `
    --name comfyui `
    --detach `
    --restart always `
    --volume C:\Users\shsuw\Documents\coding\PracticalWork\aio\comfyui\models:/opt/comfyui/models/checkpoints    --publish 8188:8188 `
    --runtime nvidia `
    --gpus all `
    ghcr.io/lecode-official/comfyui-docker:latest `
    python main.py --listen

# or python main.py --listen

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
