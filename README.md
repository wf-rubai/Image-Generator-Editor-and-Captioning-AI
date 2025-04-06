# Image Generator, Editor and Captioning AI

Run this at the terminal or at a Google Colab `Code` block to install all the needed dependencies
```bash
!pip install flask flask-cors pyngrok
!ngrok authtoken YOUR_NGROK_AUTHTOKEN
!pip install diffusers transformers torch flask flask-cors pyngrok
```
## **Note**
- Replace `YOUR_NGROK_AUTHTOKEN` with your actual ngrok authtoken.
- You can find your ngrok authtoken by signing up at [NGROK](https://ngrok.com/)
- The second `pip install` command ensures all required libraries, including machine learning frameworks like `diffusers`, `transformers`, and `torch`, are installed.
---
Than run the following code for a `Flask API`
```bash
from flask import Flask, request, send_file
from flask_cors import CORS
from pyngrok import ngrok
import torch
from diffusers import StableDiffusionPipeline

app = Flask(__name__)
CORS(app)

# Load the Stable Diffusion model
device = "cuda" if torch.cuda.is_available() else "cpu"
pipe = StableDiffusionPipeline.from_pretrained("runwayml/stable-diffusion-v1-5").to(device)

@app.route("/generate", methods=["POST"])
def generate():
    data = request.json
    prompt = data.get("prompt", "A fantasy landscape")

    # Generate image
    image = pipe(prompt).images[0]
    image_path = "generated.png"
    image.save(image_path)

    return send_file(image_path, mimetype="image/png")

# Start ngrok tunnel
public_url = ngrok.connect(5000).public_url
print("API running at:", public_url)

app.run(port=5000)
```
## **Note**
- After running this code, you will get a public URL for your `Flask API`.
- Replace `public_url` of `let response = await fetch("public_url/generate",` with this `Flask API` URL at `generateImage` JS function at the `HTML file` 