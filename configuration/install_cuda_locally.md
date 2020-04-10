# Install CUDA 10.1 + cudnn in Ubunut 
In this tutorial we will see how to install cuda locally. After reading this you well get intution about how to confiture use multiple cuda version in a same OS

## Step 1 - Remove Old Nvidia Driver
The first step is to purge currently installed Nvidia drivers so that it does not conflict with the newer versions.
```bash
sudo apt-get purge nvidia*
```

## Step 2: Install nvidia graphics driver
Add driver ppa and update system
```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
```
Search latest nvidia-drivers upported by your system
```bash
apt-cache search nvidia-driver
```
Install nvidia server based on you latest supported driver. On the time of writting this .md file latest driver version is
440
```bash
sudo apt-get install nvidia-driver-440
```
Restart your machine. Restart is important without it OS will not found nvidia-driver
```bash
sudo init 6
```

## Step 3: Check nvidia-driver installation

## Step 4 - Nvidia driver
The first we should check is that we have an Nvidia driver installed for our graphics card.we can check what graphics driver we have installed with the `nvidia-smi` command. we see some output like the following:

![](https://github.com/lab-semantics/Quick-Configuration/blob/master/img/nvidia-driver-test.png)

## Step 5: You may need to install CUDA dependencies
Run the following command
```bash
sudo apt-get install freeglut3 freeglut3-dev libxi-dev libxmu-dev
```

## Step 6: Get the CUDA run file install installer
According to your os version download CUDA run file instaler from []() 
To install this run file execute. Select continue if driver not match with recomanded driver
```bash
sudo sh cuda_10.1.105_418.39_linux.run
```
Uncheck driver version user kyeboard arro key and enter
```bash
┌──────────────────────────────────────────────────────────────────────────────┐
│ CUDA Installer                                                               │
│ - [ ] Driver                                                                 │
│      [ ] 418.39                                                              │
│ + [X] CUDA Toolkit 10.1                                                      │
│   [X] CUDA Samples 10.1                                                      │
│   [X] CUDA Demo Suite 10.1                                                   │
│   [X] CUDA Documentation 10.1                                                │
│   Install                                                                    │
│   Options                                                                    │
│                                                                              │
│ Up/Down: Move | Left/Right: Expand | 'Enter': Select | 'A': Advanced options │
└──────────────────────────────────────────────────────────────────────────────┘
```
## Step 7: Create a bash file to that activate your CUDA.
We now create a bash file. Using this we will activate our cuda version. Bash file is very usefull when you want to multiple cuda version in a same OS

I'll create the file with the name `cuda10.1-env.sh`. Add the following lines to this file,

```bash
export PATH=$PATH:/usr/local/cuda-10.1/bin
export CUDADIR=/usr/local/cuda-10.1
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-10.1/lib64
```
Change your cuda version with your ternet cuda version for example `cuda-9.0, cuda-10.0, cuda-10.1 cuda-10.2`

## Step 8: Activate your cuda env
```bash
source cuda10.1-env.sh
```

## Step 9: Install `cudnn` 

Now visit https://developer.nvidia.com/cudnn to get CUDNN Go to the downloads archive page again and find version for CUDA 10.1 that we just installed. Download the link that says `cuDNN v7.5 Library for Linux`. This will download an archive that we can unpack and move the contents the correct locations.
![](https://github.com/lab-semantics/Quick-Configuration/blob/master/img/cudnn.png)
Get the Library for Linux file for CUDA 10.1. Once downloaded,we are going to unpack the archive and move it the contents into the directory where we installed CUDA 10.1

Execute following commands in terminal of the download directory
```bash
# Unpack the archive
tar -zxvf cudnn-10.1-linux-x64-v7.tgz

# Move the unpacked contents to your CUDA directory
sudo cp -P cuda/lib64/* /usr/local/cuda-10.1/lib64/
sudo cp  cuda/include/* /usr/local/cuda-10.1/include/

# Give read access to all users
sudo chmod a+r /usr/local/cuda-10.1/include/cudnn.h
```

### Help
- [Install CUDA 10.1 in ubunut](https://www.pugetsystems.com/labs/hpc/How-To-Install-CUDA-10-1-on-Ubuntu-19-04-1405/)
- [Configure DL environment](https://github.com/menon92/Deep-Learning-Guide/blob/master/configuration/configure-deep-learning-environment-for-gpu-machine.md)
