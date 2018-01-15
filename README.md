# Ubuntu16.04-Install-Opencv2.4.13

- [環境設定1](http://blog.csdn.net/woainishifu/article/details/77449373)
- [環境設定2](http://blog.csdn.net/TKchengzi/article/details/52939526)
- [多版本共存](http://www.linuxdiyf.com/linux/30865.html)


# First Installed Method:

## Step1. Install Dependencies
```C++
$ sudo apt-get update
$ sudo apt-get install -y build-essential
$ sudo apt-get install -y cmake
$ sudo apt-get install -y libgtk2.0-dev
$ sudo apt-get install -y pkg-config
$ sudo apt-get install -y python-numpy python-dev
$ sudo apt-get install -y libavcodec-dev libavformat-dev libswscale-dev
$ sudo apt-get install -y libjpeg-dev libpng12-dev libtiff5-dev libjasper-dev
$ sudo apt-get -qq install libopencv-dev build-essential checkinstall cmake pkg-config yasm libjpeg-dev libjasper-dev libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev libxine2 libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libv4l-dev python-dev python-numpy libtbb-dev libqt4-dev libgtk2.0-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev x264 v4l-utils
```
<br/>

## Step2. Download Opencv-2.4.13
```C++
$ wget http://downloads.sourceforge.net/project/opencvlibrary/opencv-unix/2.4.13/opencv-2.4.13.zip
$ unzip opencv-2.4.13.zip
$ cd opencv-2.4.13
$ mkdir release
$ cd release
```
<br/>

## Step3. Compile and Install
```C++
$ cmake -G "Unix Makefiles" -DCMAKE_CXX_COMPILER=/usr/bin/g++ CMAKE_C_COMPILER=/usr/bin/gcc -DCMAKE_BUILD_TYPE=RELEASE -DCMAKE_INSTALL_PREFIX=/usr/local -DWITH_TBB=ON -DBUILD_NEW_PYTHON_SUPPORT=ON -DWITH_V4L=ON -DINSTALL_C_EXAMPLES=ON -DINSTALL_PYTHON_EXAMPLES=ON -DBUILD_EXAMPLES=ON -DWITH_QT=ON -DWITH_OPENGL=ON -DBUILD_FAT_JAVA_LIB=ON -DINSTALL_TO_MANGLED_PATHS=ON -DINSTALL_CREATE_DISTRIB=ON -DINSTALL_TESTS=ON -DENABLE_FAST_MATH=ON -DWITH_IMAGEIO=ON -DBUILD_SHARED_LIBS=OFF -DWITH_GSTREAMER=ON ..

$ make all -j2 # 2 cores

$ sudo make install
```
<br/>

## Step4. Environment Startup
```C++
$ sudo gedit /etc/ld.so.conf.d/opencv.conf
  /usr/local/lib  
$ sudo ldconfig

$ sudo gedit /etc/bash.bashrc 
    PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig  
    export PKG_CONFIG_PATH
-----------------------------------------------
$ su //获取root权限，否则下面的source命令不可用  
  su: Authentication failure  
$ sudo passwd root
  Enter new UNIX password:   
  Retype new UNIX password:   
  passwd: password updated successfully 
-----------------------------------------------
$ su

# source /etc/bash.bashrc 
然后按 Ctrl+d 组合键来退出root权限，接着输入下面的命令即可：

$ sudo updatedb
```
<br/>

## Step5. When Cmake . & make
```C++
$ cmake -DOpenCV_DIR=/home/ubuntu/opencv-2.4.13/release/
```

**or add CMakeLists.txt as follows:**
```C++
  set( OpenCV_DIR "/home/ubuntu/opencv-2.4.13/release/" )

  find_package(OpenCV REQUIRED)

  add_executable( ${PROJECT_NAME} XXX.cpp)

  target_link_libraries( ${PROJECT_NAME} ${OpenCV_LIBS} )  ### need after add_executable or in the end

```
```C++
$ make
```
<br/>

---

# Others:

## Make sure we have installed a C++ compiler.
```C++
$ sudo apt-get install build-essential g++
```
<br/>

## Test a simple OpenCV program. Creates a graphical window, hence you should plug a HDMI monitor in or use a remote viewer such as X Tunneling or VNC or TeamViewer on your desktop.
```C++
$ cd ~/opencv-2.4.13/samples/cpp

$ g++ edge.cpp -lopencv_core -lopencv_imgproc -lopencv_highgui -o edge
(Or for OpenCV 3.0: g++ edge.cpp -lopencv_core -lopencv_imgproc -lopencv_highgui -lopencv_imgcodecs -o edge)

$ ./edge
```
<br/>

## Test a GPU accelerated OpenCV sample.
```C++
$ cd ../gpu

$ g++ houghlines.cpp -lopencv_core -lopencv_imgproc -lopencv_highgui -lopencv_calib3d -lopencv_contrib -lopencv_features2d -lopencv_flann -lopencv_gpu -lopencv_legacy -lopencv_ml -lopencv_objdetect -lopencv_photo -lopencv_stitching -lopencv_superres -lopencv_video -lopencv_videostab -o houghlines

$ ./houghlines ../cpp/logo_in_clutter.png
```
<br/>

---
---


# Second Installed Method:
<br/>

- [reference 1](http://blog.csdn.net/woainishifu/article/details/77449373)
- [reference 2](https://www.learnopencv.com/install-opencv3-on-ubuntu/)
- [reference 3](https://docs.opencv.org/2.4/doc/tutorials/introduction/linux_install/linux_install.html#getting-opencv-source-code)

### KEEP UBUNTU OR DEBIAN UP TO DATE
```C++
$ sudo apt-get -y update
$ sudo apt-get -y upgrade
$ sudo apt-get -y dist-upgrade
$ sudo apt-get -y autoremove
```

## INSTALL THE DEPENDENCIES

### Build tools:
```C++
sudo apt-get install -y build-essential cmake
```

### GUI (if you want to use GTK instead of Qt, replace 'qt5-default' with 'libgtkglext1-dev' and remove '-DWITH_QT=ON' option in CMake):
```C++
sudo apt-get install -y qt5-default libvtk6-dev
```

### Media I/O:
```C++
sudo apt-get install -y zlib1g-dev libjpeg-dev libwebp-dev libpng-dev libtiff5-dev libjasper-dev libopenexr-dev libgdal-dev
```

### Video I/O:
```C++
sudo apt-get install -y libdc1394-22-dev libavcodec-dev libavformat-dev libswscale-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev yasm libopencore-amrnb-dev libopencore-amrwb-dev libv4l-dev libxine2-dev
```

### Parallelism and linear algebra libraries:
```C++
sudo apt-get install -y libtbb-dev libeigen3-dev
```

### Python:
```C++
sudo apt-get install -y python-dev python-tk python-numpy python3-dev python3-tk python3-numpy
```

### Java:
```C++
sudo apt-get install -y ant default-jdk
```

### Documentation:
```C++
sudo apt-get install -y doxygen
```

## INSTALL THE LIBRARY (YOU CAN CHANGE '2.4.13' FOR THE LAST STABLE VERSION)
```C++
$ sudo apt-get install -y unzip wget
$ wget https://github.com/opencv/opencv/archive/2.4.13.zip
$ unzip 2.4.13.zip
$ rm 2.4.13.zip
$ mv opencv-2.4.13 OpenCV
$ cd OpenCV
$ mkdir build
$ cd build
$ cmake -DCMAKE_BUILD_TYPE=RELEASE -DCMAKE_INSTALL_PREFIX=/usr/local -DWITH_QT=ON -DWITH_OPENGL=ON -DFORCE_VTK=ON -DWITH_TBB=ON -DBUILD_NEW_PYTHON_SUPPORT=ON -DWITH_V4L=ON INSTALL_C_EXAMPLES=ON -DINSTALL_PYTHON_EXAMPLES=ON -DBUILD_EXAMPLES=ON -DWITH_GDAL=ON -DWITH_XINE=ON -DINSTALL_TESTS=ON -DWITH_GSTREAMER=ON -DBUILD_EXAMPLES=ON ..
$ make -j4
$ sudo make install
$ sudo ldconfig
```
## Test Opencv Sample Code
```C++
./cpp-example-edge /home/ubuntu/opencv-2.4.13/samples/c/fruits.jpg
```

## error 
if show error as following:</br>
/usr/bin/ld: 找不到 -lopencv_dep_cudart </br>
then <br>

```C++
sudo ln -s /usr/local/cuda/lib64/libcudart.so /usr/lib/libopencv_dep_cudart.so
```

## EXECUTE SOME OPENCV EXAMPLES AND COMPILE A DEMONSTRATION

**To complete this step, please visit 'http://milq.github.io/install-opencv-ubuntu-debian'.**

---
## About CUDA Version: if CUDA9.0 UP Ver, create 'CMake ..' need as follow cammand:
```C++
cmake -DCMAKE_BUILD_TYPE=RELEASE -DCMAKE_INSTALL_PREFIX=/usr/local -DWITH_QT=ON -DWITH_OPENGL=ON -DWITH_TBB=ON -DBUILD_NEW_PYTHON_SUPPORT=ON -DWITH_V4L=ON INSTALL_C_EXAMPLES=ON -DINSTALL_PYTHON_EXAMPLES=ON -DBUILD_EXAMPLES=ON -DWITH_XINE=ON -DINSTALL_TESTS=ON -DWITH_GSTREAMER=ON -DWITH_CUDA=OFF -DBUILD_EXAMPLES=ON ..
```
**to close 'WITH_CUDA=OFF'**<br/>
<br/>


