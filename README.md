# Ultra Turbo Anime Generator

Production-ready image generation system using **SD-Turbo**, **SDXL-Turbo**, **ControlNet**, and **LoRA** for real-time anime-style image synthesis.

## Features

- **Dual Model Support**: SD-Turbo (speed) and SDXL-Turbo (quality)
- **ControlNet Integration**: Canny edge detection for style consistency
- **Flexible LoRA System**: Additive and sequential merge modes
- **Batch Processing**: Optimized for multiple images
- **Cross-Platform**: CUDA, MPS (Apple Silicon), and CPU support

## Requirements

### Hardware
- **Recommended**: NVIDIA GPU with 8GB+ VRAM (RTX 3070+)
- **Alternative**: Apple Silicon Mac with 16GB+ unified memory
- **Minimum**: CPU-only mode (much slower)

### Software
- Python 3.11 (recommended for best compatibility)
- Jupyter Notebook/Lab
- CUDA toolkit (for NVIDIA GPUs)

## Installation

```bash
# Create virtual environment
python -m venv env
source env/bin/activate  # On Windows: env\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

## Usage

1. **Start Jupyter (or run on your IDE)**
   ```bash
   jupyter notebook anime_generator.ipynb
   ```

2. **Run the notebook** - Execute cells in order or run all at once

3. **Customize parameters** in the `GenCfg` section:
   ```python
   cfg = GenCfg(
       prompt='high-quality anime portrait',
       negative='distorted, blur',
       steps=4,                     # 1-8 for Turbo models
       strength=0.85,               # Image-to-image strength
       use_turbo=True,              # True=SD-Turbo, False=SDXL-Turbo
       lora_mode='seq',             # 'add' (fast) or 'seq' (quality)
       seed=1234
   )
   ```

## Model Selection

### SD-Turbo
- **Speed**: Fastest (1-4 steps)
- **Resolution**: 512x512
- **VRAM**: ~4GB minimum
- **Use case**: Quick iterations

### SDXL-Turbo
- **Speed**: Fast (2-8 steps)  
- **Resolution**: 1024x1024
- **VRAM**: ~6GB minimum
- **Use case**: Higher quality output

## LoRA Modes

- **Additive (`'add'`)**: Faster, more stable, direct weight addition
- **Sequential (`'seq'`)**: Richer effects, layered application

## Input Images

Place images in `assets/example_inputs/`. Supported formats: JPEG, PNG. Images auto-resized to 512x512.

## Performance Optimization

**For Speed:**
```python
cfg = GenCfg(steps=1, use_turbo=True, lora_mode='add', guidance=1.0)
```

**For Quality:**
```python
cfg = GenCfg(steps=4, use_turbo=False, lora_mode='seq', guidance=2.0)
```

## Troubleshooting

### Out of Memory
- Reduce batch size: Change `BATCH = len(inputs)` to smaller number
- Use SD-Turbo instead of SDXL-Turbo
- Enable CPU offloading: `pipe.enable_model_cpu_offload()`

### Slow Generation
- Ensure GPU detection works
- Reduce inference steps
- Close other GPU applications

### Import Errors
```bash
pip install --upgrade -r requirements.txt
# For CUDA issues:
conda install pytorch torchvision pytorch-cuda=11.8 -c pytorch -c nvidia
```

## Expected Performance

- **RTX 4090**: SD-Turbo ~50-100ms, SDXL-Turbo ~100-200ms per image
- **RTX 3070**: SD-Turbo ~100-200ms, SDXL-Turbo ~300-500ms per image
- **M2 Max**: SD-Turbo ~500-1000ms, SDXL-Turbo ~1-2s per image

## Advanced Usage

### Custom LoRAs
1. Place `.safetensors` files in `assets/models/lora/`
2. Update `lora_paths` list in notebook

### Project Structure
```
├── anime_generator_oficial.ipynb    # Main notebook
├── requirements.txt                 # Dependencies  
└── assets/
    ├── example_inputs/              # Input images
    ├── example_outputs/             # Generated results
    └── models/lora/                 # LoRA weights
```
