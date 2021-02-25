# InstancePose: Fast-Multiple 6DoF Object Pose Estimation from a Single RGB Image

![introduction](./images/Introduction.png)

Any questions or discussions are welcomed!

## Installation

1. Set up python 3.6.9 environment

```
pip install -r requirements.txt
```

For users with a RTX GPU, you must use CUDA10 and pytorch v1.5.0 built from CUDA10.

Operating system: Ubuntu 18.04 is recommended

2. Compile some extension utils

```
ROOT=/path_to/instancepose
cd $ROOT/lib/utils/extend_utils
```

Adjust the CUDA version of `cuda_include` and `cudart` in `build_extend_utils_cffi.py` to be compatible with the CUDA in your computer.

```
sudo apt-get install libgoogle-glog-dev
sudo apt-get install libsuitesparse-dev
sudo apt-get install libatlas-base-dev
python build_extend_utils_cffi.py
```

run `build_ceres.sh` and move all files in `ceres/ceres-solver-x.xx.x/build/lib/` to `$ROOT/lib/utils/extend_utils/lib`.

```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/path_to/instancepose/lib/utils/extend_utils/lib
```

## Dataset Configuration

1. Prepare the dataset

Download the LINEMOD, which can be found [here](https://zjueducn-my.sharepoint.com/:u:/g/personal/pengsida_zju_edu_cn/EXK2K0B-QrNPi8MYLDFHdB8BQm9cWTxRGV9dQgauczkVYQ?e=beftUz).

Download the LINEMOD_ORIG, which can be found [here](https://zjueducn-my.sharepoint.com/:u:/g/personal/pengsida_zju_edu_cn/EaoGIPguY3FAgrFKKhi32fcB_nrMcNRm8jVCZQd7G_-Wbg?e=ig4aHk).

Download the OCCLUSION_LINEMOD, which can be found [here](https://zjueducn-my.sharepoint.com/:u:/g/personal/pengsida_zju_edu_cn/ESXrP0zskd5IvvuvG3TXD-4BMgbDrHZ_bevurBrAcKE5Dg?e=r0EgoA).

2. Create the soft link

```
mkdir $ROOT/data
ln -s path_to/LINEMOD $ROOT/data/LINEMOD
ln -s path_to/LINEMOD_ORIG $ROOT/data/LINEMOD_ORIG
ln -s path_to/OCCLUSION_LINEMOD $ROOT/data/OCCLUSION_LINEMOD
```

3. Synthesize images for each object

See [pvnet-rendering](https://github.com/zju-3dv/pvnet-rendering) for information about the image synthesis.

## Demo

1. Download the pretrained model from [here](https://1drv.ms/u/s!AtZjYZ01QjphgQkDZa7fyvvaD7P6)
2. put it to `$ROOT/data/model/all/`
3. Run the demo

```
python tools/demo.py
```

If setup correctly, the output will look like

![cat](./assets/cat.png)

## Training and testing

### Training with synthetic data

```
python tools/train_linemod.py --cfg_file configs/linemod_train.json --linemod_cls all
```

### Testing

```
python tools/train_linemod.py --cfg_file configs/linemod_train.json --linemod_cls all --test_model
```

## Citation

If you find this code useful for your research, please cite.
