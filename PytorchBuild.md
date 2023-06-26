### Pythorch build for jetson nano

Jetson Linux only supports python version 3.6, so if you need to use PyTorch with python 3.7 or higher, you will need to build PyTroch yourself, since the official Jetson PyTorch library does not exist.

#### Install packages

Install the necessary packages for python3.8 and PyTorch.

```
sudo apt-get update
sudo apt-get install -y \
      python3.8 python3.8-dev \
      ninja-build git cmake clang \
      libopenmpi-dev libomp-dev ccache \
      libopenblas-dev libblas-dev libeigen3-dev \
      python3-pip libjpeg-dev \
      gnupg2 curl
```

#### Install cmake

Since the default cmake is too old to build PyTorch, install cmake that supports PyTorch builds.

```
sudo apt-get install -y software-properties-common lsb-release
sudo wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor - | sudo tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null
sudo apt-add-repository "deb https://apt.kitware.com/ubuntu/ $(lsb_release -cs) main"
sudo apt-get update
sudo apt-get install -y cmake
```


#### Install CUDA and cuDNN.

To Do

#### remove exisitng numpy package

Before building PyTorch and TorchVision, remove numpy installed by apt because it causes conflicts and install numpy for python3.8.
```
sudo apt remove -y python3-numpy
```

#### build and install PyTorch with python3.8

Install the python3.8 packages, clone PyTorch and apply the patch. For more details about the patch, see this page.

```
python3.8 -m pip install -U pip
python3.8 -m pip install -U setuptools
python3.8 -m pip install -U wheel mock pillow
python3.8 -m pip install scikit-build
python3.8 -m pip install cython Pillow numpy
```

##### download PyTorch v1.11.0 with all its libraries
```
git clone -b v1.11.0 --depth 1 --recursive --recurse-submodules --shallow-submodules https://github.com/pytorch/pytorch.git
(
cd pytorch
python3.8 -m pip install -r requirements.txt
wget https://raw.githubusercontent.com/otamajakusi/build_jetson_nano_libraries/main/pytorch/pytorch-1.11-jetson.patch
patch -p1 < pytorch-1.11-jetson.patch
)
```

##### Build pytorch with max_jobs set to 2
Build PyTorch with MAX_JOBS set to 2 because the Jetson Nano is low on memory. 
Increasing MAX_JOBS will build faster, but will hang at a very high probability.

```
export BUILD_CAFFE2_OPS=OFF
export USE_FBGEMM=OFF
export USE_FAKELOWP=OFF
export BUILD_TEST=OFF
export USE_MKLDNN=OFF
export USE_NNPACK=OFF
export USE_XNNPACK=OFF
export USE_QNNPACK=OFF
export USE_PYTORCH_QNNPACK=OFF
export USE_CUDA=ON
export USE_CUDNN=ON
export TORCH_CUDA_ARCH_LIST="5.3;6.2;7.2"
export USE_NCCL=OFF
export USE_SYSTEM_NCCL=OFF
export USE_OPENCV=OFF
export MAX_JOBS=2
# set path to ccache
export PATH=/usr/lib/ccache:/usr/local/cuda/bin:$PATH
# set clang compiler
export CC=clang
export CXX=clang++
# create symlink to cublas
# ln -s /usr/lib/aarch64-linux-gnu/libcublas.so /usr/local/cuda/lib64/libcublas.so
# start the build
(
  cd pytorch
  python3.8 setup.py bdist_wheel
)
```

This build takes about 13 hours.

```
find pytorch/dist -type f|xargs python3.8 -m pip install
```

##### build and install TorchVision with python3.8

Clone and build TorchVision.
```
# torch vision
git clone --depth=1 https://github.com/pytorch/vision torchvision -b v0.12.0
(
  cd torchvision
  export TORCH_CUDA_ARCH_LIST='5.3;6.2;7.2'
  export FORCE_CUDA=1
  export MAX_JOBS=2
  python3.8 setup.py bdist_wheel
)
```

find torchvision/dist -type f|xargs python3.8 -m pip install
