# MoodSense Art Bot (Text-to-Image) Setup Guide

This guide provides step-by-step instructions to set up and run the text-to-image functionality of the MoodSense Art Bot project. The project takes a text input (e.g., a mood like "happy") and generates a mood-based image using Stable Diffusion, displaying the image directly in the notebook. The code is designed to run in a Kaggle Notebook with GPU support but can also be executed locally.

## Prerequisites

Before starting, ensure you have:
- A **GitHub account** to access the repository.
- A **Kaggle account** to create and run Notebooks (free at [kaggle.com](https://www.kaggle.com/)).
- A **Hugging Face account** for Stable Diffusion access (free at [huggingface.co](https://huggingface.co/)).
- A computer with internet access and a modern web browser (e.g., Chrome, Firefox).
- (Optional) A local machine with Python 3.8+, Jupyter Notebook, and a GPU for local execution.

## Step 1: Clone the Repository

1. **Access the Repository**:
   - Go to your GitHub repository: [https://github.com/your-username/moodsense-art-bot](https://github.com/your-username/moodsense-art-bot) (replace `your-username` with your GitHub username).
   - Ensure it contains:
     - `notebook.ipynb` (or `mood_chatbot.py`): The text-to-image code.
     - `mood_mapping.json`: Mood configuration file.
     - (Optional) `requirements.txt`: Dependencies.

2. **Clone Locally** (if running locally):
   - Install Git: [git-scm.com](https://git-scm.com/).
   - Open a terminal and run:
     ```bash
     git clone https://github.com/your-username/moodsense-art-bot.git
     cd moodsense-art-bot
     ```
   - Verify files:
     ```bash
     ls
     ```
     - Expected: `notebook.ipynb`, `mood_mapping.json`, etc.

3. **Use in Kaggle** (recommended):
   - Import the notebook directly in Kaggle (see Step 2).

## Step 2: Set Up the Environment

### Option 1: Kaggle Notebook (Recommended)

1. **Create a New Kaggle Notebook**:
   - Go to [kaggle.com](https://www.kaggle.com/) > **Code** > **New Notebook**.
   - Set:
     - **Accelerator**: **GPU T4 x2** (in **Settings** > **Accelerator**).
     - **Internet**: Enable (in **Settings** > **Internet** > **On**).

2. **Import the Notebook**:
   - Download `notebook.ipynb` from your GitHub repository:
     - Go to the repository, click `notebook.ipynb`, and click **Download** (or copy the raw content).
   - In the Kaggle Notebook:
     - Click **File** > **Upload Notebook**.
     - Upload `notebook.ipynb` or paste its content into a new cell.
   - If using `mood_chatbot.py`, copy its content into a new code cell.

3. **Upload `mood_mapping.json`**:
   - Download `mood_mapping.json` from the GitHub repository.
   - In the Kaggle Notebook:
     - Click **+ Add Data** (right sidebar).
     - Select **Upload** > Drag and drop `mood_mapping.json` or browse to upload.
     - Move it to `/kaggle/working`:
       ```python
       !cp /kaggle/input/your-dataset/mood_mapping.json /kaggle/working/mood_mapping.json
       ```
       - Replace `your-dataset` with the uploaded dataset name.

### Option 2: Local Machine

1. **Install Python and Jupyter**:
   - Install Python 3.8+: [python.org](https://www.python.org/).
   - Install Jupyter Notebook:
     ```bash
     pip install jupyter
     ```
   - Verify:
     ```bash
     python --version
     jupyter notebook --version
     ```

2. **Install GPU Drivers (Optional)**:
   - For Stable Diffusion, install NVIDIA GPU drivers, CUDA, and cuDNN if you have a compatible GPU:
     - Download from [nvidia.com](https://www.nvidia.com/).
     - Follow installation guides for your OS.

3. **Navigate to Project Directory**:
   ```bash
   cd moodsense-art-bot
   ```

## Step 3: Install Dependencies

### In Kaggle

1. **Install Python Dependencies**:
   - Add a code cell and run:
     ```python
     !pip install ipywidgets diffusers transformers torch pillow numpy
     ```
   - This takes ~1–2 minutes.

2. **Verify Dependencies**:
   - Ensure no errors in the output. Re-run the cell if errors occur.

### Locally

1. **Install Dependencies**:
   - If `requirements.txt` is present:
     ```bash
     pip install -r requirements.txt
     ```
   - Otherwise, install manually:
     ```bash
     pip install ipywidgets diffusers transformers torch pillow numpy
     ```

## Step 4: Configure Secrets

1. **Hugging Face Token**:
   - Go to [huggingface.co](https://huggingface.co/) > Profile > Settings > Access Tokens.
   - Create a new token (Read access) and copy it (e.g., `hf_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`).
   - **In Kaggle**:
     - Go to **Add-ons** > **Secrets** (top-right).
     - Add:
       - **Name**: `HF_TOKEN`
       - **Value**: Your Hugging Face token.
       - Click **Save**.
   - **Locally**:
     - Set as an environment variable:
       ```bash
       export HF_TOKEN="your_hugging_face_token"
       ```
       - On Windows (Command Prompt):
         ```cmd
         set HF_TOKEN=your_hugging_face_token
         ```

## Step 5: Prepare `mood_mapping.json`

- Ensure `mood_mapping.json` is in the project directory or `/kaggle/working`. If missing, create it:
  ```python
  %%writefile /kaggle/working/mood_mapping.json
  {
    "happy": {
      "colors": ["yellow", "orange"],
      "lighting": "bright, sunny",
      "style": "vibrant, cartoon"
    },
    "sad": {
      "colors": ["blue", "gray"],
      "lighting": "dim, overcast",
      "style": "muted, photorealistic"
    },
    "calm": {
      "colors": ["light blue", "pastel green"],
      "lighting": "soft, diffused",
      "style": "serene, watercolor"
    },
    "angry": {
      "colors": ["red", "black"],
      "lighting": "harsh, dramatic",
      "style": "bold, cinematic"
    },
    "excited": {
      "colors": ["bright red", "yellow"],
      "lighting": "dynamic, vivid",
      "style": "energetic, illustrative"
    }
  }
  ```
- Run the cell (Kaggle) or save as `mood_mapping.json` (local).

## Step 6: Run the Code

### In Kaggle

1. **Copy Code**:
   - Open `notebook.ipynb` in the Kaggle Notebook editor.
   - If using `mood_chatbot.py`, copy its content into a new code cell:
     ```python
     # Paste content of mood_chatbot.py here
     ```

2. **Execute All Cells**:
   - Click **Run All** or run each cell:
     - Install dependencies (~1–2 minutes).
     - Create `mood_mapping.json`.
     - Run the main code (~2–5 minutes for model download, ~20–30 seconds per image).
   - Expected startup output:
     ```
     MoodSense Art Bot interface loaded.
     ```

3. **Interact with the Interface**:
   - A widget interface will appear with:
     - **Mood**: Text input (e.g., enter `happy`).
     - **Generate Image**: Button to process the input.
   - **Test Text Input**:
     - Enter `happy` in the **Mood** field.
     - Click **Generate Image**.
     - Expected output:
       ```
       Processing text mood: happy
       ```
       - The generated image will display below the interface in the notebook.

### Locally

1. **Run Jupyter Notebook**:
   ```bash
   jupyter notebook
   ```
   - Open `notebook.ipynb` in the browser.
   - Alternatively, run `mood_chatbot.py`:
     ```bash
     python mood_chatbot.py
     ```

2. **Interact with the Interface**:
   - Enter a mood (e.g., `happy`) and generate the image.
   - The image will display in the notebook or script output.

## Step 7: Troubleshoot Common Issues

1. **Dependency Errors**:
   - Ensure internet is enabled in Kaggle Settings.
   - Re-run:
     ```python
     !pip install ipywidgets diffusers transformers torch pillow numpy
     ```

2. **Hugging Face Token Error**:
   - Verify the token in Kaggle Secrets or environment variable.
   - Regenerate at [huggingface.co](https://huggingface.co/) if invalid.

3. **Image Not Generating**:
   - Ensure GPU is enabled (Kaggle: GPU T4 x2; local: NVIDIA GPU with CUDA).
   - Check `mood_mapping.json` exists and is valid JSON.
   - Verify the mood input (e.g., `happy`, `sad`) matches keys in `mood_mapping.json`.
   - Test image generation:
     ```python
     from diffusers import StableDiffusionPipeline
     pipe = StableDiffusionPipeline.from_pretrained("runwayml/stable-diffusion-v1-5", torch_dtype=torch.float16, use_auth_token="your_hf_token")
     pipe = pipe.to("cuda")
     image = pipe("a happy scene, yellow tones, bright lighting, vibrant style").images[0]
     image.save("test.png")
     ```

4. **Widget Interface Not Displaying**:
   - Ensure `ipywidgets` is installed and enabled:
     ```python
     !jupyter nbextension enable --py widgetsnbextension
     ```
   - Restart the notebook kernel and re-run.

## Additional Notes

- **Kaggle Limitations**: The free tier has GPU usage limits. Monitor usage in Kaggle to avoid quotas.
- **Local Execution**: Requires a powerful GPU for Stable Diffusion. Without a GPU, image generation may be slow or fail.
- **Repository Updates**: If you update the code, re-download `notebook.ipynb` or `mood_chatbot.py` from GitHub and repeat the setup.

For further assistance, contact the repository owner or open an issue on GitHub.