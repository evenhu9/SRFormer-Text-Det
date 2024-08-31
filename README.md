<h1 align="center"> SRFormer: Text Detection Transformer with Incorporated Segmentation and Regression </h1> 

<p align="center">
<a href="https://arxiv.org/abs/2308.10531"><img src="https://img.shields.io/badge/arXiv-Paper-<color>"></a>
<a href="https://paperswithcode.com/sota/scene-text-detection-on-total-text?p=srformer-empowering-regression-based-text"><img src="https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/srformer-empowering-regression-based-text/scene-text-detection-on-total-text"></a>
<a href="https://paperswithcode.com/sota/scene-text-detection-on-scut-ctw1500?p=srformer-empowering-regression-based-text"><img src="https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/srformer-empowering-regression-based-text/scene-text-detection-on-scut-ctw1500"></a>
<a href="https://paperswithcode.com/sota/scene-text-detection-on-ic19-art?p=srformer-empowering-regression-based-text"><img src="https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/srformer-empowering-regression-based-text/scene-text-detection-on-ic19-art"></a>
</p>

This is the official repo for the paper "SRFormer: Text Detection Transformer with Incorporated Segmentation and Regression".

## Introduction

<img src="./figs/srformer.jpg" alt="image" style="zoom:40%;" />

**Abstract.** Existing techniques for text detection can be broadly classified into two primary groups: segmentation-based methods and regression-based methods. 
Segmentation models offer enhanced robustness to font variations but require intricate post-processing, leading to high computational overhead. Regression-based methods undertake instance-aware prediction but face limitations in robustness and data efficiency due to their reliance on high-level representations. In our academic pursuit, we propose SRFormer, a unified DETR-based model with amalgamated Segmentation and Regression, aiming at the synergistic harnessing of the inherent robustness in segmentation representations, along with the straightforward post-processing of instance-level regression. Our empirical analysis indicates that favorable segmentation predictions can be obtained in the initial decoder layers. In light of this, we constrain the incorporation of segmentation branches to the first few decoder layers and employ progressive regression refinement in subsequent layers, achieving performance gains while minimizing additional computational load from the mask. Furthermore, we propose a Mask-informed Query Enhancement module, where we take the segmentation result as a natural soft-ROI to pool and extract robust pixel representations to diversify and enhance instance queries. Extensive experimentation across multiple benchmarks has yielded compelling findings, highlighting our method's exceptional robustness, superior training and data efficiency, as well as its state-of-the-art performance.

## Updates
**12/09/2023:**:tada: Our paper is accepted to AAAI'24

**08/21/2023:** Core code \& checkpoints uploaded

**08/28/2023:** Update data preparation

## Main Results

|Benchmark|Backbone|Precision|Recall|F-measure|Pre-trained Model|Fine-tuned Model|
|:------:|:------:|:------:|:------:|:------:|:------:|:------:|
|Total-Text|Res50|92.2|87.9|90.0|[OneDrive](https://1drv.ms/u/s!AtF4kB5K12hqgUcE1EhRK17fFhNf?e=Yg1ula)|[Seg#1](https://1drv.ms/u/s!AtF4kB5K12hqgUnwxi_vVITCBG-w?e=2tQnYB); [Seg#2](https://1drv.ms/u/s!AtF4kB5K12hqgT8xV85-mDo6_Ino?e=Vf6hp2); [Seg#3](https://1drv.ms/u/s!AtF4kB5K12hqgUiPObZeQvYoAjnb?e=MD6UTo)|
|CTW1500|Res50|91.6|87.7|89.6|Same as above $\uparrow$ |[Seg#2](https://1drv.ms/u/s!AtF4kB5K12hqgU3EyDwv7-CDr8KY?e=H0606E); [Seg#3](https://1drv.ms/u/s!AtF4kB5K12hqgUz3PqgiXdoEH7kw?e=tHIeIg)|
|ICDAR19 ArT|Res50|86.2|73.4|79.3|[OneDrive](https://1drv.ms/u/s!AtF4kB5K12hqgUvaI9K329gHzQzz?e=mWeERS)|[Seg#1](https://1drv.ms/u/s!AtF4kB5K12hqgUpcOheps7ztstF1?e=Dw0KXA)|

## Usage

It's recommended to configure the environment using Anaconda. Python 3.10 + PyTorch 1.13.1 + CUDA 11.3 + Detectron2 are suggested.

- ### Installation
```
conda create -n SRFormer python=3.10 -y
conda activate SRFormer
pip install torch==1.13.1+cu116 torchvision==0.14.1+cu116 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/cu116
pip install opencv-python scipy timm shapely albumentations Polygon3 pyclipper
python -m pip install 'git+https://github.com/facebookresearch/detectron2.git'
pip install setuptools==59.5.0

cd SRFormer-Text-Detection
python setup.py build develop
```
- ### Data Preparation

>**SynthText-150K & MLT & LSVT (images):**  [Source](https://github.com/aim-uofa/AdelaiDet/tree/master/datasets) 
>
>**Total-Text (including rotated images)**: [OneDrive](https://1drv.ms/u/s!AimBgYV7JjTlgccOW1TUlgm64M0yRA?e=jwY6b1)
>
>**CTW1500 (including rotated images)**: [OneDrive](https://1drv.ms/u/s!AimBgYV7JjTlgccPGEv4DkiUl23MEg?e=44CtL6)
>
>**ICDAR19 ArT (including rotated images)**: [OneDrive](https://1drv.ms/u/s!AtF4kB5K12hqgVufZu390bfiaHMf?e=20VN6D)
>
>**Validation Set of MLT17 categorized by language:** [OneDrive](https://1drv.ms/u/s!AtF4kB5K12hqgXCzVB5JUNSW3wRP?e=IrydBa)
>
>**Annotations for training and evaluation:** [OneDrive](https://1drv.ms/u/s!AtF4kB5K12hqgVr0wHEUD7XzVePw?e=EKcBDC)
>
**Organize your data as follows:** 
```
|- datasets
   |- syntext1
   |  |- train_images
   |  └─ train_poly_pos.json  
   |- syntext2
   |  |- train_images
   |  └─ train_poly_pos.json
   |- mlt
   |  |- train_images
   |  └─ train_poly_pos.json
   |- valid_mlt
   |  |- All
   |  |- Arabic
   |  |- Bangla
   |  |- Chinese
   |  |- Japanese
   |  |- Korean
   |  |- Latin
   |  |- Arabic_test.json
   |  |- Bangla_test.json
   |  |- Chinese_test_json
   |  |- Japanese_test.json
   |  |- Korean_test.json
   |  |- Latin_test.json
   |  └─ mlt_valid_test.json
   |- totaltext
   |  |- test_images_rotate
   |  |- train_images_rotate
   |  |- test_poly.json
   |  |─ train_poly_pos.json
   |  └─ train_poly_rotate_pos.json
   |- ctw1500
   |  |- test_images
   |  |- train_images_rotate
   |  |- test_poly.json
   |  └─ train_poly_rotate_pos.json
   |- lsvt
   |  |- train_images
   |  └─ train_poly_pos.json
   |- art
   |  |- test_images
   |  |- train_images_rotate
   |  |- test_poly.json
   |  |─ train_poly_pos.json
   |  └─ train_poly_rotate_pos.json
   |- evaluation
   |  |- *.zip
```

- ### Training
  
**Step 0:** You should first set [SEG_LAYERS](https://github.com/retsuh-bqw/SRFormer-Text-Det/blob/8e45d1434a708d2988323fef56f161d167affdcd/configs/SRFormer/Base.yaml#L26C12-L26C12) in *configs/SRFormer/base.yaml*  to determine the number of decoder layers incorporated in the Segmentation & Regression chunk. For more detailed information, please refer to our paper.


**1. Pre-train:**
To pre-train the model for Total-Text and CTW1500, the config file should be `configs/SRformer/Pretrain/R_50_poly.yaml`. For ICDAR19 ArT, please use `configs/SRFormer/Pretrain_ArT/R_50_poly.yaml`. Please adjust the GPU number according to your situation.

```
python tools/train_net.py --config-file ${CONFIG_FILE} --num-gpus 8
```

**2. Fine-tune:**
With the pre-trained model, use the following command to fine-tune it on the target benchmark. The pre-trained models are also provided.  For example:

```
python tools/train_net.py --config-file configs/SRFormer/TotalText/R_50_poly.yaml --num-gpus 8
```

- ### Evaluation
```
python tools/train_net.py --config-file ${CONFIG_FILE} --num-gpus ${NUM_GPUS} --eval-only
```
For ICDAR19 ArT, a file named `art_submit.json` will be saved in `output/r_50_poly/art/finetune/inference/`. The json file can be directly submitted to [the ICDAR19-ArT website](https://rrc.cvc.uab.es/?ch=14) for evaluation.

- ### Inference & Visualization
```
python demo/demo.py --config-file ${CONFIG_FILE} --input ${IMAGES_FOLDER_OR_ONE_IMAGE_PATH} --output ${OUTPUT_PATH} --opts MODEL.WEIGHTS <MODEL_PATH>
```

## Citation

If you find our work useful or inspiring, please consider citing:
```
@inproceedings{bu2024srformer,
  title={SRFormer: Text Detection Transformer with Incorporated Segmentation and Regression},
  author={Bu, Qingwen and Park, Sungrae and Khang, Minsoo and Cheng, Yichuan},
  booktitle={Proceedings of the AAAI Conference on Artificial Intelligence},
  volume={38},
  number={2},
  pages={855--863},
  year={2024}
}
```

## Acknowledgement

SRFormer is inspired a lot by and [TESTR](https://github.com/mlpc-ucsd/TESTR) and [DPText-DETR](https://github.com/ymy-k/DPText-DETR). Thanks for their great works!
