# Setup DeepLearning development environment

## Step -1 :
[Check GPU deep learning performance comparison](https://lambdalabs.com/blog/2080-ti-deep-learning-benchmarks/)

## Step 0 - Tensorflow, CUDA, cuDNN Nvidia graphivs deriver version Build Configuration. 
Check comptable version of these to each other

Check out table : https://www.tensorflow.org/install/source#tested_build_configurations


## Step 1 - Remove Old Nvidia Driver

The first step is to purge currently installed Nvidia drivers so that it does not conflict with the newer versions.

```bash
sudo apt-get purge nvidia*
```
## Step 2 - Install Latest Nvidia Driver
Check out nvidia unix driver archrive: https://www.nvidia.com/object/unix.html also check which drive version support your graphics card.Now enable the graphics-drivers PPA to your system. Currently, it supports `Ubuntu 18.04 LTS, 17.10, 17.04, 16.04 LTS, and 14.04 LTS` operating systems. Keep in mind that its still under testing phase.

```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
```
Execute the below command to install Nvidia graphics driver on your system.
```bash
sudo apt update
sudo apt install nvidia-390
sudo init 6
```
## Step 3 - Verify Nvidia Driver
After installation reboot your system, So that your desktop load the new Nvidia driver. Now run below command to check Nvidia module.

```bash
lsmod | grep nvidia
```

# Install Tensorflow GPU on Ubuntu 16.04LTS, 18.04 LTS

## Step 4 - Nvidia driver
The first we should check is that we have an Nvidia driver installed for our graphics card.we can check what graphics driver we have installed with thenvidia-smicommand. we should see some output like the following:
![](https://github.com/lab-semantics/Quick-Configuration/blob/master/img/nvidia-driver-test.png)
The driver version we have installed is near the top left next to `NVIDIA-SMI`. I’ve got `nvidia-390.67` installed.

## Step 5 - CUDA Toolkit 9.0
Got to https://developer.nvidia.com/cuda-toolkit-archive in the Archived Releases select CUDA Toolkit 9.0 (Sept 2017).Then download the runfile for Ubuntu 16.04 LTS,18.04 LTS

![](https://github.com/lab-semantics/Quick-Configuration/blob/master/img/cuda-toolkit.png)
It ll be something like this. download the file from the Base Installer link.

Once you’ve finished downloading that file, navigate to where the file was downloaded in your terminal.usually it ll be in your downloads directory.So navigate to the download directory and right click there and click open terminal.

In the terminal execute following two commands one by one

```bash
sudo chmod +x cuda_9.0.176_384.81_linux.run
./cuda_9.0.176_384.81_linux.run --override
```

Accept the terms and conditions, say `yes` to installing with an unsupported configuration.and no to `Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 384.81?`. Make sure you don’t agree to install the new driver.Follow the prompts to install the toolkit using the default install locations.

## Step 6 - CUDA post-install actions[`PATH Configuration`]
So Tensorflow can find our CUDA installation and use it properly, we need to add these lines to the end of you `~/.bashrc`

first in our terminal type following command to open `~/.bashrc` :

```bash
nano ~/.bashrc
```
Then add following paths at the end of the file `~/.bashrc`
```bash
export PATH=/usr/local/cuda-9.0/bin${PATH:+${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:${LD_LIBRARY_PATH:+${LD_LIBRARY_PATH}}
```
finally In order to activate the installation, we should source the ~/.bashrc file:

```bash
source ~/.bashrc
```
## Step 7 - Install CUDNN 7.0
Now visit https://developer.nvidia.com/cudnn to get CUDNN 7.0. Go to the downloads archive page again and find version 7.0 for CUDA 9.0 that we just installed. Download the link that says `cuDNN v7.0.5 Library for Linux`. This will download an archive that we can unpack and move the contents the correct locations.

![](https://github.com/lab-semantics/Quick-Configuration/blob/master/img/cudnn.png)
Get the Library for Linux file for CUDA 9.0.

Once downloaded,we are going to unpack the archive and move it the contents into the directory where we installed CUDA 9.0:

execute following commands in terminal of the download directory

```bash
# Unpack the archive
tar -zxvf cudnn-9.0-linux-x64-v7.tgz

# Move the unpacked contents to your CUDA directory
sudo cp -P cuda/lib64/* /usr/local/cuda-9.0/lib64/
sudo cp  cuda/include/* /usr/local/cuda-9.0/include/

# Give read access to all users
sudo chmod a+r /usr/local/cuda-9.0/include/cudnn.h
```
## Step 8 - Install libcupti
simply execute following command
```bash
sudo apt-get install libcupti-dev
```
## Step 9 - Installing Anaconda
The best way to install Anaconda is to download (https://www.anaconda.com/download/#linux) the latest Anaconda installer bash script, verify it, and then run it.

Navigate download directory.Now we can run the script:
```bash
bash Anaconda3-5.0.1-Linux-x86_64.sh
```

follow all prompts an install.Once it’s complete we’ll receive the following output:

```bash
...
installation finished.
Do you wish the installer to prepend the Anaconda3 install location
to PATH in your /home/kekayan/.bashrc ? [yes|no]
[no] >>>
```
Type `yes` so that we can use the `conda` command.

In order to activate the installation, you should source the `~/.bashrc` file:
```bash
source ~/.bashrc
```
Once we have done that, you can verify our installation by making use of the `conda` command, for example with `list`:
```bash
conda list
```
Let’s create our virtual environment.so we can install tensorflow there
```bash
conda create --name tf
```
tf is the name i have for my environment. Then we can activate it by
```bash
source activate tf
```
Next we intsall pip by following command
```bash
easy_install -U pip
```
## Setp 10 - Install Tensorflow
finally install tensorflow gpu by issuing following command
```bash
pip3 install --upgrade tensorflow-gpu
```
run the following code to check tensorflow is using gpu or not
```python
import tensorflow as tf

# Creates a graph.
a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a')
b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b')
c = tf.matmul(a, b)
# Creates a session with log_device_placement set to True.
sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
# Runs the op.
print(sess.run(c))
```
You should see the following output:
```python
Device mapping:
/job:localhost/replica:0/task:0/device:GPU:0 -> device: 0, name: Tesla K40c, pci bus
id: 0000:05:00.0
b: /job:localhost/replica:0/task:0/device:GPU:0
a: /job:localhost/replica:0/task:0/device:GPU:0
MatMul: /job:localhost/replica:0/task:0/device:GPU:0
[[ 22.  28.]
 [ 49.  64.]]
```
chekc the tensorflow wibsite https://www.tensorflow.org/guide/using_gpu

## Tested on Nvidia Geforce GTX 1080 Ti

### Reference

* https://websiteforstudents.com/install-proprietary-nvidia-gpu-drivers-on-ubuntu-16-04-17-10-18-04/
* https://medium.com/codezillas/step-by-step-guide-to-install-tensorflow-gpu-on-ubuntu-18-04-lts-6feceb0df5c0
* https://www.tensorflow.org/guide/using_gpu
