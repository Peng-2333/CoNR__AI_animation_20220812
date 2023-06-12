[English](https://github.com/megvii-research/CoNR/blob/main/README.md) | [中文](https://github.com/megvii-research/CoNR/blob/main/README_chinese.md)
# Collaborative Neural Rendering using Anime Character Sheets

Our paper is accepted by [Special Track of IJCAI2023](https://ijcai-23.org/special-track-on-ai-the-arts-and-creativity/), the revision of the paper is [available](https://arxiv.org/abs/2207.05378). 

2023/4/18: The dataset is now avaliable in [CoNR_Dataset](https://github.com/P2Oileen/CoNR_Dataset)! 🎉

## [Demo Page](https://transpchan.github.io/live3d/) | Colab [English](https://colab.research.google.com/github/megvii-research/CoNR/blob/main/notebooks/conr.ipynb)/[中文](https://colab.research.google.com/github/megvii-research/CoNR/blob/main/notebooks/conr_chinese.ipynb) | [arXiv](https://arxiv.org/abs/2207.05378) | [BiliBili](https://www.bilibili.com/video/BV19V4y1x7bJ/) | [YouTube](https://youtu.be/Z4HXWBF7mLI) | [CoNR_Dataset](https://github.com/P2Oileen/CoNR_Dataset)

![image](images/1.gif) ![image](images/2.gif)
![image](images/MAIN.png)

## Introduction

This project is the official implement of [Collaborative Neural Rendering using Anime Character Sheets](https://arxiv.org/abs/2207.05378), which aims to genarate vivid dancing videos from hand-drawn anime character sheets (ACS).[^1] Watch more demos and details in our firmly recommended video in [BiliBili](https://www.bilibili.com/video/BV19V4y1x7bJ/) or [YouTube](https://youtu.be/Z4HXWBF7mLI). Our [FAQ on Zhihu (in Chinese)](https://zhuanlan.zhihu.com/p/565391665) explains the ideas underpinning CoNR.

## Usage

#### Prerequisites

* NVIDIA GPU + CUDA + CUDNN
* Python 3.6

#### Installation

* Clone this repository

```bash
git clone https://github.com/megvii-research/CoNR
```

* Dependencies

To install all the dependencies, please run the following commands.

```bash
cd CoNR
pip install -r requirements.txt
```

* Download Weights
Download weights from Google Drive. Alternatively, you can download from [Baidu Netdisk](https://pan.baidu.com/s/1U11iIk-DiJodgCveSzB6ig?pwd=RDxc) (password:RDxc).

```
mkdir weights && cd weights
gdown https://drive.google.com/uc?id=1M1LEpx70tJ72AIV2TQKr6NE_7mJ7tLYx
gdown https://drive.google.com/uc?id=1YvZy3NHkJ6gC3pq_j8agcbEJymHCwJy0
gdown https://drive.google.com/uc?id=1AOWZxBvTo9nUf2_9Y7Xe27ZFQuPrnx9i
gdown https://drive.google.com/uc?id=19jM1-GcqgGoE1bjmQycQw_vqD9C5e-Jm
```

#### Prepare Inputs
We provide two Ultra-Dense Pose sequences for two characters. You can generate more UDPs via 3D models and motions refers to [our paper](https://arxiv.org/abs/2207.05378), or use [MMD2UDP](https://github.com/KurisuMakise004/MMD2UDP)(Thanks to @KurisuMakise004 ).
[Baidu Netdisk](https://pan.baidu.com/s/1hWvz4iQXnVTaTSb6vu1NBg?pwd=RDxc) (password:RDxc) 

```
# for short hair girl
gdown https://drive.google.com/uc?id=11HMSaEkN__QiAZSnCuaM6GI143xo62KO
unzip short_hair.zip
mv short_hair/ poses/

# for double ponytail girl
gdown https://drive.google.com/uc?id=1WNnGVuU0ZLyEn04HzRKzITXqib1wwM4Q
unzip double_ponytail.zip
mv double_ponytail/ poses/
```

We provide sample inputs of anime character sheets. You can also draw more by yourself.
Character sheets need to be cut out from the background and in png format.
[Baidu Netdisk](https://pan.baidu.com/s/1shpP90GOMeHke7MuT0-Txw?pwd=RDxc) (password:RDxc) 

```
# for short hair girl
gdown https://drive.google.com/uc?id=1r-3hUlENSWj81ve2IUPkRKNB81o9WrwT
unzip short_hair_images.zip
mv short_hair_images/ character_sheet/

# for double ponytail girl
gdown https://drive.google.com/uc?id=1XMrJf9Lk_dWgXyTJhbEK2LZIXL9G3MWc
unzip double_ponytail_images.zip
mv double_ponytail_images/ character_sheet/
```

#### RUN!
* with web UI (powered by [Streamlit](https://streamlit.io/))

```
streamlit run streamlit.py --server.port=8501
```
then open your browser and visit `localhost:8501`, follow the instructions to generate video.

* via terminal

```
mkdir {dir_to_save_result}

python -m torch.distributed.launch \
--nproc_per_node=1 train.py --mode=test \
--world_size=1 --dataloaders=2 \
--test_input_poses_images={dir_to_poses} \
--test_input_person_images={dir_to_character_sheet} \
--test_output_dir={dir_to_save_result} \
--test_checkpoint_dir={dir_to_weights}

ffmpeg -r 30 -y -i {dir_to_save_result}/%d.png -r 30 -c:v libx264 output.mp4 -r 30
```

## Citation
```bibtex
@inproceedings{lin2023conr,
  title={Collaborative Neural Rendering using Anime Character Sheets},
  author={Lin, Zuzeng and Huang, Ailin and Huang, Zhewei},
  booktitle = {Proceedings of the Thirty-First International Joint Conference on Artificial Intelligence (IJCAI)},
  year={2023}
}
```

[^1]: Zuzeng Lin is not involved in the submissions to AAAI23, and IJCAI23, which are done by other authors alone, after seeing the heartbreaking reviews from CVPR22 and ECCV22.
He explored the idea of [assisting anime creation with AI](https://zhuanlan.zhihu.com/p/565391665) and proposed CoNR as a baseline to solve consistency and artistic control issues at the end of the year 2020.  Other authors revised his draft to its present state and made demo videos after he quit, but they are not involved in the [successor versions](https://github.com/transpchan/Live3D-v2). He appreciates the discussion with many people who are interested in this project, and Live3D public beta users in Sept. 2021.
