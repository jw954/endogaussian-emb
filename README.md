# EndoGaussian: Gaussian Splatting for Deformable Surgical Scene Reconstruction

## arXiv Preprint

### [Project Page]()| [arXiv Paper]()


[Yifan Liu](https://guanjunwu.github.io/)<sup>1*</sup>, [Chenxin Li](https://github.com/taoranyi)<sup>1*</sup>,
[Chen Yang](https://jaminfong.cn/)<sup>2</sup>, [Yixuan Yuan](http://lingxixie.com/)<sup>1✉</sup>

<sup>1</sup>Department of Electronic Engineering, CUHK &emsp; <sup>2</sup>Department of Electrical Engineering &emsp;

<sup>\*</sup> Equal Contributions. <sup>✉</sup> Corresponding Author. 

-------------------------------------------


## This repo has been transferred to [Here](https://github.com/CUHK-AIM-Group/EndoGaussian).



## Commands to run the code

### Get correct CUDA version (If you get error)

```
export CUDA_HOME=/usr/local/cuda-11.8
export PATH=$CUDA_HOME/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=$CUDA_HOME/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```


### Environment Setup

```
git submodule update --init --recursive

conda create -n EndoGaussian python=3.10

conda activate EndoGaussian

pip install -r requirements.txt

pip install -e submodules/depth-diff-gaussian-rasterization

pip install -e submodules/simple-knn
```

### Get the embeddings

```
cd encoders/sam_encoder
pip install -e .
```
Download the following same encoders

ViT-H: https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth

ViT-L: https://dl.fbaipublicfiles.com/segment_anything/sam_vit_l_0b3195.pth

ViT-B: https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth

Place it in `encoders/sam_encoder/checkpoints`

```
cd encoders/sam_encoder
python export_image_embeddings.py --checkpoint checkpoints/sam_vit_h_4b8939.pth --model-type vit_h --input <dataset_path>/images  --output <data_path>/sam_embeddings
```


### For training:

Use below to try training directly on endonerf data (using feature3dgs train script)

```
python train.py -s ../pulling_soft_tissues --port 6017 --expname endonerf/pulling --configs arguments/endonerf/pulling.py
```


### Rendering

```
python render.py --model_path output/endonerf/pulling  --skip_train --skip_video --configs arguments/endonerf/pulling.py
```


### Evaluation

```
python metrics.py --model_path output/endonerf/pulling
```


expected folder structure 

```
train 
 --images
 --sam_embeddings
 --sparse/0
   --cameras.bin
   --images.bin
   --points3D.bin
   --points3D.ply
   --project.ini
```
