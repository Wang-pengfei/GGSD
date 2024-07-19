# GGSD
Official PyTorch codes for "Open Vocabulary 3D Scene Understanding via Geometry Guided Self-Distillation", ECCV2024

## Note！！
Please note that the code has not been fully organized yet. The current code cannot be executed directly. I will remove this note once I have finished organizing it.



## Installation
Our code is based on [OpenScene](https://github.com/pengsongyou/openscene/blob/main/installation.md), and you can install the environment according to OpenScene or the following commands.

Start by cloning the repo:
```bash
git clone https://github.com/Wang-pengfei/GGSD.git
cd GGSD
```

First of all, you have to make sure that you have all dependencies in place.
The simplest way to do so, is to use [anaconda](https://www.anaconda.com/). 

You can create an anaconda environment called `GGSD` as below. For linux, you need to install `libopenexr-dev` before creating the environment.

```bash
sudo apt-get install libopenexr-dev # for linux
conda create -n GGSD python=3.8
conda activate GGSD
```

Step 1: install PyTorch (we tested on 1.7.1, but the following versions should also work):

```bash
pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 torchaudio==0.7.2 -f https://download.pytorch.org/whl/torch_stable.html
```

Step 2: install MinkowskiNet:

```bash
sudo apt install build-essential python3-dev libopenblas-dev
```
If you do not have sudo right, try the following:
```
conda install openblas-devel -c anaconda
```
And now install MinkowskiNet:
```
pip install -U git+https://github.com/NVIDIA/MinkowskiEngine -v --no-deps \
                           --install-option="--force_cuda" \
                           --install-option="--blas=openblas"
```
If it is still giving you error, please refer to their [official installation page](https://github.com/NVIDIA/MinkowskiEngine#installation).


Step 3: install all the remaining dependencies:
```bash
pip install -r requirements.txt
```

Step 4 (optional): if you need to run multi-view feature fusion with OpenSeg (especially for your own dataset), remember to install:
```bash
pip install tensorflow
```

## Data Preparation

We provide the **pre-processed 3D&2D data** and **multi-view fused features** for the following datasets:
- ScanNet
- Matterport3D
- nuScenes
- Replica
### Pre-processed 3D&2D Data
One can download the pre-processed datasets by running the script below, and following the command line instruction to download the corresponding datasets:
```bash
bash scripts/download_dataset.sh
```
The script will download and unpack data into the folder `data/`. One can also download the dataset somewhere else, but link to the corresponding folder with the symbolic link:
```bash
ln -s /PATH/TO/DOWNLOADED/FOLDER data
```

**Note**: 2D processed datasets (e.g. `scannet_2d`) are only needed if you want to do multi-view feature fusion on your own. If so, please follow the [instruction for multi-view fusion](./scripts/feature_fusion/README.md).

### Multi-view Fused Features

You can run the following to directly download provided fused features:

```bash
bash scripts/download_fused_features.sh
```

### Pre-processed superpoint Data
will released


## Run
When you have installed the environment and obtained the **processed 3D data** and **multi-view fused features**, you are ready to run our OpenScene disilled/ensemble model for 3D semantic segmentation, or distill your own model from scratch.

### Distillation
- Start distilling:
```sh run/distill.sh EXP_NAME CONFIG.yaml```

### Evaluation
```bash
# Run 3D distilled model
sh run/eval.sh out/replica_openseg config/replica/ours_openseg_pretrained.yaml distill
```

## Acknowledgement
We build our code on top of the [OpenScene repository]([https://github.com/wbhu/BPNet](https://github.com/pengsongyou/openscene)).



