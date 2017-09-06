# caffe-installation-tutorial
This toturial tells you how to install Nvidia driver, CUDA, cuDNN, OpenCV and Caffe on ubuntu16.04. 

<h1 align = "center">Caffe installation tutorial</h1>

## Introduction<br>
&emsp;This tutorial is to guide you install Nvidia driver, CUDA, cuDNN, OpenCV and Caffe on your Ubuntu 16.04 platform. The versions of which I have tested are listed as following:<br><br>

- Linux platform: Ubuntu 16.04.03 LTS + Nvidia GTX1050
- NVIDIA-Linux-x86_64-384.59
- CUDA_8.0.61.375.26
- cudnn-8.0-linux-x64-v7
- opencv-3.3.0
- Caffe <br><br>

 &emsp; It's necessary to mention that it's just my own configuration, the versions of CUDA and cudnn would be determined by your Nvidia type. You can use the command `lspci | grep -i vga` to check your NVIDIA GPU card. The following context would tell you how to select the version of those platforms. The tutotial is also suitable for you if you just want to install Nvidia driver or opencv3.3<br>

 ## Sturcture
 &emsp;Our tutorials have a consistent structure composed of the following sections:<br>
- Title
- Introduction
- Step 1- Install dependencies
- Step 2- Install Nvida driver
- Step 3- Install CUDA
- Step 4- Install cuDNN
- Step 5- Install opencv
- Step 6- Install caffe
- Step 7- Test caffe examples

## Step 1 -- Install dependencies
##### 1.1 Install dependencies for opencv    
```command
    sudo apt-get install build-essential    
    sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev    
    sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev  libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev    
    sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev liblapacke-dev    
    sudo apt-get install libxvidcore-dev libx264-dev        
    sudo apt-get install libatlas-base-dev gfortran    
    sudo apt-get install ffmpeg    
```
##### 1.2 Install other related dependencies
```command
    sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev     libopencv-dev libhdf5-serial-dev protobuf-compiler    
    sudo apt-get install --no-install-recommends libboost-all-dev    
    sudo apt-get install libopenblas-dev liblapack-dev libatlas-base-dev    
    sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev    
```

## Step 2 -- Install Nvidia driver    
##### 2.1 Search NVIDIA driver    
We can firstly go to official [NVIDIA driver download page](http://www.nvidia.com/Download/index.aspx) and select the corresponding parameters of your platform. Here is my selected page.Click on the search button and it would turn to download page, then we can click to download the driver,the document will be downloaded to ***Downloads folder*** in home folder.<br><br> 
##### 2.2 Install NVIDIA driver<br>
&emsp;Firstly, we would add the original vga into blacklist. In the command window, input `sudo gedit /etc/modprobe.d/blacklist.conf` , input your password and then in the last line add `blacklist nouveau`, save it and then input `sudo update-initramfs -u` in the command window, finally, restart your conputer.<br>
&emsp;Most importantly, we should install driver under text interface, we can transfer graphical interface into text interface through `Ctrl+Alt+F1~F6`. You should input your usrname and password to login in. <br>
Then input `sudo service lightdm stop`, then we can install Nvidia driver.<br>
- Give the .run file execution permission
```
    cd ~/Downloads
    sudo chmod a+x NVIDIA-Linux-x86_64-375.20.run
```
- Installation (Pay attention to the parameters)
```
    sudo ./NVIDIA-Linux-x86_64-375.20.run –no-x-check –no-nouveau-check –no-opengl-files
```    
&emsp;Following the guide to finish installation, finally reboot your system. <br>    
&emsp;**Note:** `If you have the problem in infinitely loop login, you can use the command above to avoid this problem. It's useful.`   <br><br>
&emsp;You can use the command `sudo nvidia-smi` to check if the nvidia driver is correctly installed. If the GPU information is listed such as the following, it means your nvidia driver is correctly installed.<br><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;

## Step 3 -- Install CUDA    
##### 3.1 Download CUDA    
Firstly, we would go to [CUDA toolkit Download](https://developer.nvidia.com/cuda-downloads) page. Select your target platform, my platform is Ubuntu16.04 64bit, the following image is my choice.<br><br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;  
The .run file will be downloaded to ***Downloads folder*** in home folder.
##### 3.2 Install CUDA  
```command
    cd ~/Downloads
    sudo chmod 777 cuda_8.0.61_375.26_linux.run
    sudo ./cuda_8.0.61_375.26_linux.run --override
```    
when one choice come out to let you install another version of nvidia driver, you should choose **no**. Other choices could be default.    
After that, we could configure the environment. Input the command `sudo gedit ~/.bashrc`, and write the following text into the file.
```
    export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```
## Step 4 -- Install cuDNN  
##### 4.1 Download cuDNN    
Firstly, we go to the [cuDNN download page](https://developer.nvidia.com/rdp/form/cudnn-download-survey), it's necesscry to mention that it need membership tho download cuDNN, you can register freely and finish some survey then you can download cuDNN.    
<br>![cudnn](../Pictures/cudnn.png)    
<br>The compressed file would be downloaded to ***Downloads folder*** in home folder. Uncompressed the file, you can get a file named *cuda*.
##### 4.2 Configure cuDNN   
```
    cd cuda/include
    sudo cp -P cudnn.h /usr/local/cuda-8.0/include
    sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda-8.0/lib64/
    sudo chmod a+r /usr/local/cuda-8.0/lib64/libcudnn*
```    
Conduct the following command to configure cudnn:    
```
    cd /usr/local/cuda-8.0/lib64/
    sudo rm -rf libcudnn.so libcudnn.so.7
    sudo ln -s libcudnn.so.7.0.1 libcudnn.so.7
    sudo ln -s libcudnn.so.7 libcudnn.so
```    
Using the command `ls -al | grep libcudnn` to check if the soft link is correctly set. If the result shows as following, it's alright.    
<br>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;   
<br>Then we set some environment variables:    
`sudo gedit /etc/profile` and write `export PATH=/usr/local/cuda/bin:$PATH` into it.    
`sudo vim /etc/ld.so.conf.d/cuda.conf`, write `/usr/local/cuda/lib64` into it and then `sudo ldconfig` to make the link validly.    
##### 4.3 Test    
```
    cd /usr/local/cuda-8.0/samples/1_Utilities/deviceQuery
    sudo make
    sudo ./deviceQuery
```
If the result is as following, it means the CUDA and cuDNN are correctly installed.       

## Step 5 -- Install OpenCV
We could go to the official [opencv  releases](http://opencv.org/releases.html) website to select the version of opencv. I would download the 3.3.0 source code. If you haven't install cmake yet, please install it firstly. Then we would compile the source code and install it:        
```
    cd ~/opencv
    mkdir build
    cd build
    sudo cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local ..
    sudo make -j8
    sudo make install
```       
## Step 6 -- Install Caffe    
We could go to [Caffe github](https://github.com/BVLC/caffe) to download the source code and unpressed it.    
Firstly, we would revise Makefile.config file:
```
    cd caffe
    sudo cp Makefile.config.example Makefile.config
    sudo gedit Makefile.config
```    
- If you would like to use cuDNN, you should revise `#USE_CUDNN := 1` to `USE_CUDNN := 1`
- If the version of opencv is 3, you should revise `#OPENCV_VERSION := 3` to `OPENCV_VERSION := 3`
- If you would like to write layer with python layer,you should revise `#WITH_PYTHON_LAYER := 1` to `WITH_PYTHON_LAYER := 1`
- Most importantly, revise the following
```
    INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include
    LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib
```    
to     
```
    INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial
    LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial     
```    
Then revise the Makefile file:  
Change the 145 line :
```
    NVCCFLAGS +=-ccbin=$(CXX) -Xcompiler-fPIC $(COMMON_FLAGS)
```
to  
```
    NVCCFLAGS += -D_FORCE_INLINES -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)
```
Then edit `/usr/local/cuda/include/host_config.h`:  comment `#error-- unsupported GNU version! gcc versions later than 4.9 are not supported!` to `//#error-- unsupported GNU version! gcc versions later than 4.9 are not supported!`    

Then it comes to important step-compile files:  
```
    mkdir build
    cd build
    cmake ..
    make all -j8
    sudo make runtest
```   
It may cost a lot of time to make and make runtest. When all files pass the test, it means your caffe has been correctly installed.   

## Step 7 -- Test Caffe Examples
We will test Caffe using the classical mnist example in caffe example folder. And you can follow the steps in `~/caffe/examples/mnist/readme.md`.
Firstly,you would like to download the data and convert tho special format:
```
    cd caffe
    vim ./data/mnist/get_mnist.sh
    ./data/mnist/get_mnist.sh
```
You can run `./example/mnist/train_lenet.sh` to train your example. The default network layers are in `caffe/examples/mnist/lenet_train_test.prototxt`, and you can change GPU mode and other solver parameters `caffe/examples/mnist/lenet_solver.prototxt`. My test accuracy is up to 0.9907.

