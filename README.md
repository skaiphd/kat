# Kolmogorov–Arnold Transformer: A PyTorch Implementation

<p align="center">
<a href="https://arxiv.org/abs/2405.07992" alt="arXiv">
    <img src="https://img.shields.io/badge/arXiv-2405.07992-b31b1b.svg?style=flat" /></a>
<a href="https://colab.research.google.com/drive/1DTJRsPczV0pOwmFhEjSWyI2NqQoR_u-K?usp=sharing" alt="Colab">
    <img src="https://colab.research.google.com/assets/colab-badge.svg" /></a>
</p>

<p align="center">
<img src="assets/KAT.png" width="400"> <br>
</p>

This is a PyTorch/GPU implementation of the paper **Kolmogorov–Arnold Transformer (KAT)**, which replace the MLP layers in vision transformer with KAN layers.

**Kolmogorov–Arnold Transformer**

 📝[[Paper](https://arxiv.org/abs/2407.06182)] </>[[code](https://github.com/Adamdad/kat)]

Xingyi Yang, Xinchao Wang

National University of Singapore

### Key Insight:

The KAT model integrates KANs into transformers for large-scale training scenarios such as ImageNet, showing significant performance improvements.

## Installation

```shell
# install torch and other things
pip install timm==1.0.3
git clone https://github.com/Adamdad/rational_kat_cu.git
cd rational_kat_cu
pip install -e .
```
please refer to `https://github.com/Adamdad/rational_kat_cu.git` for the cuda rational function installation

## Model Checkpoints
Download pre-trained models or access training checkpoints:

|Model | Param| Top1 |Link|
| ---|---|---| ---|
|KAT-T| 5.7M | 74.6| [link](https://github.com/Adamdad/kat/releases/download/checkpoint/kat_small_patch16_224_32487885cf13d2c14e461c9016fac8ad43f7c769171f132530941e930aeb5fe2.pth)
|KAT-T (Pretrained)| 5.7M | 75.7| [link](https://github.com/Adamdad/kat/releases/download/checkpoint/kat_tiny_patch16_224-finetune_64f124d003803e4a7e1aba1ba23500ace359b544e8a5f0110993f25052e402fb.pth) 
|KAT-S| 22.1M | 81.2| [link](https://github.com/Adamdad/kat/releases/download/checkpoint/kat_tiny_patch16_224_1f3ad3b2e69821f3d412f2924cf159a0e266f142d739cb68f68f796f5a0fe289.pth)
|KAT-S (Pretrained)|22.1M | 82.0| [link](https://github.com/Adamdad/kat/releases/download/checkpoint/kat_small_patch_224-finetune_3ae087a4c28e2993468eb377d5151350c52c80b2a70cc48ceec63d1328ba58e0.pth)
| KAT-B|86.6M| 82.3 | [link](https://github.com/Adamdad/kat/releases/download/checkpoint/kat_base_patch16_224_abff874d925d756d15cde97303f772a3460ddbd44b9c53fb9ce5cf15be230fb6.pth)
|  KAT-B (Pretrained)|86.6M| 82.7 | |

## Model Training

```shell
bash scripts/train_kat_tiny_8x128.sh
```

If you want to change the hyper-parameters, can edit
```shell
#!/bin/bash
DATA_PATH=/local_home/dataset/imagenet/

bash ./dist_train.sh 8 $DATA_PATH \
    --model kat_tiny_swish_patch16_224 \
    -b 128 \
    --opt adamw \
    --lr 1e-3 \
    --weight-decay 0.05 \
    --epochs 300 \
    --mixup 0.8 \
    --cutmix 1.0 \
    --sched cosine \
    --smoothing 0.1 \
    --drop-path 0.1 \
    --aa rand-m9-mstd0.5 \
    --remode pixel --reprob 0.25 \
    --amp \
    --crop-pct 0.875 \
    --mean 0.485 0.456 0.406 \
    --std 0.229 0.224 0.225 \
    --model-ema \
    --model-ema-decay 0.9999 \
    --output output/kat_tiny_swish_patch16_224 \
    --log-wandb
```


## Acknowledgments
We extend our gratitude to the authors of [rational_activations]() for their contributions to CUDA rational function implementations that inspired parts of this work.