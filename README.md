
<div align="center">
<h1>IDM-VTON: Improving Diffusion Models for Authentic Virtual Try-on in the Wild</h1>

<a href='https://idm-vton.github.io'><img src='https://img.shields.io/badge/Project-Page-green'></a>
<a href='https://arxiv.org/abs/2403.05139'><img src='https://img.shields.io/badge/Paper-Arxiv-red'></a>
<a href='https://huggingface.co/spaces/yisol/IDM-VTON'><img src='https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Demo-yellow'></a>
<a href='https://huggingface.co/yisol/IDM-VTON'><img src='https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Model-blue'></a>


</div>

This is the official implementation of the paper ["Improving Diffusion Models for Authentic Virtual Try-on in the Wild"](https://arxiv.org/abs/2403.05139).

Star ⭐ us if you like it!

---


![teaser2](assets/teaser2.png)&nbsp;
![teaser](assets/teaser.png)&nbsp;


## TODO LIST


- [x] demo model
- [x] inference code
- [x] training code



## Requirements

```
git clone https://github.com/yisol/IDM-VTON.git
cd IDM-VTON

conda env create -f environment.yaml
conda activate idm
```

## Data preparation

### VITON-HD
You can download VITON-HD dataset from [VITON-HD](https://github.com/shadow2496/VITON-HD).

After download VITON-HD dataset, move vitonhd_test_tagged.json into the test folder, and move vitonhd_train_tagged.json into the train folder.

Structure of the Dataset directory should be as follows.

```

train
|-- image
|-- image-densepose
|-- agnostic-mask
|-- cloth
|-- vitonhd_train_tagged.json

test
|-- image
|-- image-densepose
|-- agnostic-mask
|-- cloth
|-- vitonhd_test_tagged.json

```

### DressCode
You can download DressCode dataset from [DressCode](https://github.com/aimagelab/dress-code).

We provide pre-computed densepose images and captions for garments [here](https://kaistackr-my.sharepoint.com/:u:/g/personal/cpis7_kaist_ac_kr/EaIPRG-aiRRIopz9i002FOwBDa-0-BHUKVZ7Ia5yAVVG3A?e=YxkAip).

We used [detectron2](https://github.com/facebookresearch/detectron2) for obtaining densepose images, refer [here](https://github.com/sangyun884/HR-VITON/issues/45) for more details.

After download the DressCode dataset, place image-densepose directories and caption text files as follows.

```
DressCode
|-- dresses
    |-- images
    |-- image-densepose
    |-- dc_caption.txt
    |-- ...
|-- lower_body
    |-- images
    |-- image-densepose
    |-- dc_caption.txt
    |-- ...
|-- upper_body
    |-- images
    |-- image-densepose
    |-- dc_caption.txt
    |-- ...
```


## Training


### Preparation

Download pre-trained ip-adapter for sdxl(IP-Adapter/sdxl_models/ip-adapter-plus_sdxl_vit-h.bin) and image encoder(IP-Adapter/models/image_encoder) [here](https://github.com/tencent-ailab/IP-Adapter).

```
git clone https://huggingface.co/h94/IP-Adapter
```

Move ip-adapter to ckpt/ip_adapter, and image encoder to ckpt/image_encoder

Start training using python file with arguments,

```
accelerate launch train_xl.py \
    --gradient_checkpointing --use_8bit_adam \
    --output_dir=result --train_batch_size=6 \
    --data_dir=DATA_DIR
```

or, you can simply run with the script file.

```
sh train_xl.sh
```


## Inference


### VITON-HD

Inference using python file with arguments,

```
accelerate launch inference.py \
    --width 768 --height 1024 --num_inference_steps 30 \
    --output_dir "result" \
    --unpaired \
    --data_dir "DATA_DIR" \
    --seed 42 \
    --test_batch_size 2 \
    --guidance_scale 2.0
```

or, you can simply run with the script file.

```
sh inference.sh
```

### DressCode

For DressCode dataset, put the category you want to generate images via category argument,
```
accelerate launch inference_dc.py \
    --width 768 --height 1024 --num_inference_steps 30 \
    --output_dir "result" \
    --unpaired \
    --data_dir "DATA_DIR" \
    --seed 42 
    --test_batch_size 2
    --guidance_scale 2.0
    --category "upper_body" 
```

or, you can simply run with the script file.
```
sh inference.sh
```

## Start a local gradio demo <a href='https://github.com/gradio-app/gradio'><img src='https://img.shields.io/github/stars/gradio-app/gradio'></a>

Download checkpoints for human parsing [here](https://huggingface.co/spaces/yisol/IDM-VTON-local/tree/main/ckpt).

Place the checkpoints under the ckpt folder.
```
ckpt
|-- densepose
    |-- model_final_162be9.pkl
|-- humanparsing
    |-- parsing_atr.onnx
    |-- parsing_lip.onnx

|-- openpose
    |-- ckpts
        |-- body_pose_model.pth
    
```




Run the following command:

```python
python gradio_demo/app.py
```






## Acknowledgements


Thanks [ZeroGPU](https://huggingface.co/zero-gpu-explorers) for providing free GPU.

Thanks [IP-Adapter](https://github.com/tencent-ailab/IP-Adapter) for base codes.

Thanks [OOTDiffusion](https://github.com/levihsu/OOTDiffusion) and [DCI-VTON](https://github.com/bcmi/DCI-VTON-Virtual-Try-On) for masking generation.

Thanks [SCHP](https://github.com/GoGoDuck912/Self-Correction-Human-Parsing) for human segmentation.

Thanks [Densepose](https://github.com/facebookresearch/DensePose) for human densepose.



## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=yisol/IDM-VTON&type=Date)](https://star-history.com/#yisol/IDM-VTON&Date)



## Citation
```
@article{choi2024improving,
  title={Improving Diffusion Models for Authentic Virtual Try-on in the Wild},
  author={Choi, Yisol and Kwak, Sangkyung and Lee, Kyungmin and Choi, Hyungwon and Shin, Jinwoo},
  journal={arXiv preprint arXiv:2403.05139},
  year={2024}
}
```

1.环境配置好电脑上安装python，安装git

2.找一个空白目录克隆  git clone https://github.com/yisol/IDM-VTON.git

3.进入克隆目录创建虚拟环境 python -m env env  

4.激活虚拟环境  env\Scripts\activate

5.安装配置文件 pip install -r requirements.txt 

在当前文件下新建立一个  requirements.txt  文件然后复制下面内容粘贴
```
accelerate==0.25.0
torchmetrics==1.2.1
tqdm==4.66.1
transformers==4.36.2
diffusers==0.25.0
einops==0.7.0
bitsandbytes==0.39.0
scipy==1.11.1
opencv-python
gradio==4.24.0
fvcore
cloudpickle
omegaconf
pycocotools
basicsr
av
onnxruntime==1.16.2
```
6.安装pytorch：  pip install torch==2.1.0 torchvision==0.16.0 torchaudio==2.1.0 –index-url https://download.pytorch.org/whl/cu118  

7.运行   python gradio_demo/app.py

## License
The codes and checkpoints in this repository are under the [CC BY-NC-SA 4.0 license](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode).


