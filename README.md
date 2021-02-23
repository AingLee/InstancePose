# InstancePose: Fast-Multiple 6DoF Object Pose Estimation from a Single RGB Image

![introduction](./images/Introduction.png)

Any questions or discussions are welcomed!

## Installation

1. Set up python 3.6.9 environment

```
pip install -r requirements.txt
```

We need compile several files, which works fine with pytorch v1.5.0 and gcc 5.4.0.

For users with a RTX GPU, you must use CUDA10 and pytorch v1.5.0 built from CUDA10.

2. Compile some extension utils

```
cd $ROOT/lib/utils/extend_utils
```

Revise the `cuda_include` and `dart` in `build_extend_utils_cffi.py` to be compatible with the CUDA in your computer.

```
sudo apt-get install libgoogle-glog-dev
sudo apt-get install libsuitesparse-dev
sudo apt-get install libatlas-base-dev
python build_extend_utils_cffi.py
```

run `build_ceres.sh` and move all files in `ceres/ceres-solver-x.xx.x/build/lib/` to `lib/utils/extend_utils/lib`.

Add LD_LIBRARY_PATH under ```$ROOT/lib/utils/extend_utils```

```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/path_to/instancepose/lib/utils/extend_utils/lib
```

## Dataset Configuration

### Prepare the dataset

Download the LINEMOD, which can be found at [here](https://zjueducn-my.sharepoint.com/:u:/g/personal/pengsida_zju_edu_cn/EXK2K0B-QrNPi8MYLDFHdB8BQm9cWTxRGV9dQgauczkVYQ?e=beftUz).

Download the LINEMOD_ORIG, which can be found at [here](https://zjueducn-my.sharepoint.com/:u:/g/personal/pengsida_zju_edu_cn/EaoGIPguY3FAgrFKKhi32fcB_nrMcNRm8jVCZQd7G_-Wbg?e=ig4aHk).

Download the OCCLUSION_LINEMOD, which can be found at [here](https://zjueducn-my.sharepoint.com/:u:/g/personal/pengsida_zju_edu_cn/ESXrP0zskd5IvvuvG3TXD-4BMgbDrHZ_bevurBrAcKE5Dg?e=r0EgoA).

### Create the soft link

```
mkdir $ROOT/data
ln -s path/to/LINEMOD $ROOT/data/LINEMOD
ln -s path/to/LINEMOD_ORIG $ROOT/data/LINEMOD_ORIG
ln -s path/to/OCCLUSION_LINEMOD $ROOT/data/OCCLUSION_LINEMOD
```

### Compute FPS keypoints

```
python lib/utils/data_utils.py
```

### Synthesize images for each object

See [pvnet-rendering](https://github.com/zju-3dv/pvnet-rendering) for information about the image synthesis.

## Demo

Download the pretrained model of cat from [here](https://1drv.ms/u/s!AtZjYZ01QjphgQkDZa7fyvvaD7P6) and put it to `$ROOT/data/model/cat_demo/199.pth`. 

Run the demo

```
python tools/demo.py
```

If setup correctly, the output will look like

![cat](./assets/cat.png)

### Visualization of the voting procedure

We add a jupyter notebook [visualization.ipynb](./visualization.ipynb) for the keypoint detection pipeline of PVNet, aiming to make it easier for readers to understand our paper. Thanks for Kudlur, M 's suggestion. 

## Training and testing

### Training on the LINEMOD

Before training, remember to add the `lib` under `extend_utils` to the LD_LIDBRARY_PATH

```
export LD_LIDBRARY_PATH=$LD_LIDBRARY_PATH:/path/to/pvnet/lib/utils/extend_utils/lib
```

Training

```
python tools/train_linemod.py --cfg_file configs/linemod_train.json --linemod_cls cat
```

### Testing

We provide the pretrained models of each object, which can be found at [here](https://1drv.ms/f/s!AtZjYZ01QjphgQBQDQghxjbkik5f).

Download the pretrained model and move it to `$ROOT/data/model/{cls}_linemod_train/199.pth`. For instance

```
mkdir $ROOT/data/model
mv ape_199.pth $ROOT/data/model/ape_linemod_train/199.pth
```

Testing

```
python tools/train_linemod.py --cfg_file configs/linemod_train.json --linemod_cls cat --test_model
```

## Citation

If you find this code useful for your research, please use the following BibTeX entry.

```
@inproceedings{peng2019pvnet,
  title={PVNet: Pixel-wise Voting Network for 6DoF Pose Estimation},
  author={Peng, Sida and Liu, Yuan and Huang, Qixing and Zhou, Xiaowei and Bao, Hujun},
  booktitle={CVPR},
  year={2019}
}
```

## Acknowledgement

This work is affliated with ZJU-SenseTime Joint Lab of 3D Vision, and its intellectual property belongs to SenseTime Group Ltd.

```
Copyright (c) ZJU-SenseTime Joint Lab of 3D Vision. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
