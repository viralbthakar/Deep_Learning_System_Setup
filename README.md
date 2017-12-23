# Deep_Learning_System_Setup
This repo is a step by step guide to setup your deep learning system from scratch. It includes every step required to setup your deep learning work station e.g. Drives, Anaconda Python and various Deep Learning Frameworks. This guide is based on my personal experience and some portions from various resources available on Internet. 

## System Specifications 
UBUNTU 16.04
NVIDIA GTX 1060

## Step - 1 : Install Updates and Other Packages Required
* First we will update our OS and install essential packages like cmake, g++, gfortran, autoremove etc. 
```
sudo apt-get update
sudo apt-get upgrade  
sudo apt-get install build-essential cmake g++ gfortran 
sudo apt-get install git pkg-config python-dev 
sudo apt-get install software-properties-common wget
sudo apt-get autoremove 
sudo rm -rf /var/lib/apt/lists/*
```

## Step - 2 : Install NVIDIA Driver

* In this section the first step is to know which GPU we are having. We can know it by executing following command. 
```
lspci | grep -i nvidia 
```

* Next step is to install the compatible drivers for our NVIDIA GPU. We can download the drivers from NVIDIA official website, but as we are on Ubuntu we will install by Terminal so that in future if you are setting up any Ubuntu Server you can also use the same steps. 

To do this we will add the [proprietary repository of Nvidia drivers](https://launchpad.net/~graphics-drivers/+archive/ubuntu/ppa). While I am writing this notes, nvidia-387 is the current official release and nvidia-384 is the long lived branch release. I will install nvidia-384 using following set of commands. 

```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-384
```

* Once the installation is complete we will restart the computer by 
```
sudo shutdown -r now
```

* After the restart we can cross check the installation by executing following command. 
```
cat /proc/driver/nvidia/version
```

## Step - 3 : CUDA Toolkit Installation 
* After the installation of Nvidia Drivers next important part is to install Cuda Toolkit. While writing this tutorial the recent relese of Cuda Toolkit is Cuda 9 but I will stick to installation with Cuda 8. Visit the page of [Cuda 8](https://developer.nvidia.com/cuda-80-ga2-download-archive) and select the approprate versions. (Make sure you download a deb local file)

The package file is about 2GB, so it may take a while to download. Installation steps are the same as that on their website, but writing it here again so one may copy and paste. Note, the installation takes a significant amount of time.

Following command for "dpkg" may change as per your download file and also you need to change directory in terminal to the directory where you have downloaded the file. 

```
sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda
```

* Add CUDA to the environment variables
```
echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

* After sourcing the bashrc file, the CUDA version can be verified using
```
nvcc -V
```

* Restart your computer
```
sudo shutdown -r now
```

## Step - 4 : Install cuDNN
* cuDNN is a GPU accelerated library for DNNs. It can help speed up execution in many cases. To be able to download the cuDNN library, you need to register in the Nvidia website at https://developer.nvidia.com/cudnn. This can take anywhere between a few hours to a couple of working days to get approved.

At the time of writing this tutorial Nvidia has officially released cuDNN 7 but on the official instruction of tensorflow installation it has mentioned that it supports cuDNN 6 currently. So I will cover steps for cuDNN 6 and I think similar steps should work for cuDNN 7 also. 

So after the registartion [download](https://developer.nvidia.com/rdp/cudnn-download) cuDNN v6.0 Library for Linux and compatible with Cuda 8.0. Considering the downloaded file stored in Downloads directory of Ubuntu use following set of instaructions to setup your cuDNN. 

```
cd ~/Downloads/
tar xvf cudnn*.tgz
cd cuda
sudo cp */*.h /usr/local/cuda/include/
sudo cp */libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

## Step - 5 : Install Anaconda Python and Create Conda Environment

* Next step is to install [Anaconda Python](https://www.anaconda.com/download/). We will be downloading Python 3.6 version of Anaconda Python. 

After the Installation completes Downloading a .sh file for Ubuntu use following command to install Anaconda. 

```
bash ~/Downloads/Anaconda3-5.0.1-Linux-x86_64.sh
```

## Step - 6 : Create Conda Environment with Specific Packages
* Next we will create Conda Environment using specific packages we want. 

To do that download the setup.yml file and use the following command to create the new environment. 

```
conda env create -f setup.yml
```

## Step - 7 : Install OpenBlas

OpenBLAS is a linear algebra library and is faster than Atlas. This step is optional, but note that some of the following steps assume that OpenBLAS is installed. You'll need to install gfortran to compile it.

```
mkdir ~/git
cd ~/git
git clone https://github.com/xianyi/OpenBLAS.git
cd OpenBLAS
make FC=gfortran -j $(($(nproc) + 1))
sudo make PREFIX=/usr/local install

echo 'export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH' >> ~/.bashrc
```


