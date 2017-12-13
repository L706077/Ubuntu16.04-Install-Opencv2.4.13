# Ubuntu16.04-Install-Opencv2.4.13

- [環境設定1](http://blog.csdn.net/woainishifu/article/details/77449373)
- [環境設定2](http://blog.csdn.net/TKchengzi/article/details/52939526)
- [多版本共存](http://www.linuxdiyf.com/linux/30865.html)



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
$ source /etc/bash.bashrc 

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
$ cd ~/opencv-2.4.9/samples/cpp

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
