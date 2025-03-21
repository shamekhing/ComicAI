# ComicAI
Shamekh Al Suwi - AI M.Sc - Practical Work - JKU Linz

## Overview
ComicAI is a project focused on visualizing tabletop role-playing game narratives, particularly Dungeons & Dragons (DnD). The goal is to bridge the gap between oral storytelling and digital visualization by summarizing and structuring game dialogue into compelling visual stories.

### Objectives
- Summarize game dialogue data
- Organize it into storytelling structures (e.g., Hero’s Journey)
- Create a visualization to enhance narrative immersion

## Prerequisites    
- Python 3.12.9  
- Docker  

## Installation & Setup

```bash
# Install the venv requirements
python -m venv comicai 
comicai\Scripts\activate 
# deactivate if needed
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

# ComfyUI for NVIDIA users (In a separate terminal run)
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

# Text-To-Speech service by Edge Browser
docker run -d -p 5050:5050  -e API_KEY=usability10  travisvn/openai-edge-tts:latest
```

The previous commands are the foundation of our UI. From her we need to set ComfyUI and OpenwebUI from their own UIs

### ComfyUI
(Optional) Go to http://localhost:8188/ in your browser for standalone image generation 

Read more about the models used [here](https://stability.ai/learning-hub/setting-up-and-using-sd3-medium-locally) and download them from [here](https://huggingface.co/ckpt/stable-diffusion-3-medium/tree/main)

1. Go to ComicAI\ComfyUI\models 
2. Place the model [sd3_medium_incl_clips](https://huggingface.co/ckpt/stable-diffusion-3-medium/resolve/main/sd3_medium_incl_clips.safetensors?download=true) checkpoint(s) in both the [models/checkpoints](./ComfyUI/models/checkpoints/) and models/unet directories of ComfyUI. Alternatively, you can create a symbolic link between models/checkpoints and models/unet to ensure both directories contain the same model checkpoints.
2. 


## Connecting the Components
After setting up the necessary services, follow these steps to connect them via OpenWebUI:

1. Open OpenWebUI in a browser (`localhost:3000`) and navigate to **Admin Panel → Settings**.
2. Enable **ComfyUI** for image workflow generation (`http://host.docker.internal:8188`).
3. Connect **Ollama** in the connection tab (`http://host.docker.internal:11434`) and disable OpenAI API.
4. Under the **Web Search** tab, configure Searxng (`http://host.docker.internal:32768/search?q=<query>`).
5. Under the **Audio** tab, set up text-to-speech (`http://host.docker.internal:5050/v1`) with API key (`usability10`).

## Features
- **Narrative Summarization**: Extracts key dialogue and structures them using storytelling techniques.
- **Real-time Visualizations**: Generates images for DnD narratives.
- **Speech-to-Text & Text-to-Speech**: Converts spoken dialogue into text and vice versa.
- **Search Aggregation**: Uses a metasearch engine for enhanced narrative research.

## Image Generation 
Refer to https://docs.openwebui.com/tutorials/images documentation on the matter.
### WorkSpace
1. Import tools into the tools tab 
2. import models into the models tab 


## Challenges & Future Work
### Current Limitations
- The Hero’s Journey framework is difficult to implement at scale.
- Improvements needed in **data segmentation** and **prompt generation**.

### Future Enhancements
- Real-time DnD session generation
- Improved narrative analysis
- Monetization opportunities

## Conclusion
Inspired by Carl Sagan’s philosophy: *"To create an apple pie from scratch, you must first create the universe."* Similarly, ComicAI aimed for a mini GPT to transform raw gameplay data into immersive visual narratives.
 