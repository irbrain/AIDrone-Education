### 1. Install Libraries

<br/>

     sudo apt update  &&  sudo apt upgrade -y     
     sudo apt install build-essential cmake git pkg-config libjpeg-dev libtiff-dev libpng-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev  libxvidcore-dev libx264-dev  libgtk2.0-dev libgtk-3-dev imagemagick libv4l-dev libatlas-base-dev gfortran python3-dev libopenblas-dev python3-pip

<br/>

### 2. Change SWAPSIZE to 2048

<br/>

       sudo dphys-swapfile swapoff
       
       sudo nano /etc/dphys-swapfile

<bt/>

![image](https://github.com/user-attachments/assets/1beb6134-5fe6-456f-94fe-a48627fcde77)

       sudo  dphys-wapfile  setup

       sudo  dphys-wapfile  swapon
       
<br/>

![image](https://github.com/user-attachments/assets/9e22ae3b-588c-40d1-8732-50a163778078)

<br/>
       

### 3. Install Opencv  (Simple waw for C++ and Python) 

     sudo apt install libopencv-dev

     sudo apt install python3-opencv

#### Check up The OpenCV's version

![image](https://github.com/user-attachments/assets/24f83ae2-74d2-4950-8dfb-87fd68dec429)

<br/>

### 4. Install OpneCV by Source  (If using  C/C++ and Python )
<br/>

       mkdir opencv
   
       cd  opencv
   
       git clone https://github.com/opencv/opencv.git
   
       git clone https://github.com/opencv/opencv_contrib.git

       cd  ~/opencv/opencv

       mkdir build

       cd build

       cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=OFF -DWITH_LIBV4L=ON -D WITH_IPP=OFF -D WITH_1394=OFF -D BUILD_WITH_DEBUG_INFO=OFF -D BUILD_DOCS=OFF -D  INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=OFF -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D ENABLE_NEON=ON -D WITH_QT=OFF -D WITH_GTK=ON -D WITH_OPENGL=ON -D  OPENCV_ENABLE_NONFREE=ON -D  OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules -D WITH_V4L=ON -D WITH_FFMPEG=ON -D WITH_XINE=ON -D ENABLE_PRECOMPILED_HEADERS=OFF -D BUILD_NEW_PYTHON_SUPPORT=ON -D  OPENCV_GENERATE_PKGCONFIG=ON -D  PYTHON3_PACKAGES_PATH=/usr/lib/python3.11/dist-packages  ../

  <br/>
  
       make –j4

       sudo make install

       sudo  ldconfig

<br/>

### 5. Install MJPG-Streamer in Rasbperry Pi Zero 2w 

<br/>

####  Build  mjpg-streamer source  

     sudo apt update && sudo apt upgrade -y      

     git clone https://github.com/jacksonliam/mjpg-streamer.git

     cd mjpg-streamer/mjpg-streamer-experimental

     make

     sudo make install

     cd ~     
    
<br/>  
     
####  Edit mjpg-streamer.service  

     sudo nano /etc/systemd/system/mjpg-streamer.service
 
     < change from user id to your id >
    
<br/> 

     [Unit]
     
     Description=mjpg-streamer     
     After=network-online.target

     [Service]
     
     ExecStart=/home/user id/mjpg-streamer/mjpg-streamer-experimental/mjpg_streamer -i "/home/user id/mjpg-streamer/mjpg-streamer-experimental/input_uvc.so -d /dev/video0 -r 640x480 -f 30" -o 
       "/home/user id/mjpg-streamer/mjpg-streamer-experimental/output_http.so -w /home/user id/mjpg-streamer/mjpg-streamer-experimental/www"
     
     Restart=always     
     User= user id

     [Install]
     
     WantedBy=multi-user.target

<br/>
               
####  Start  mjpg-streamer service 

    sudo systemctl daemon-reload
   
    sudo systemctl enable mjpg-streamer

    sudo systemctl start mjpg-streamer

<br/>

####  You can see the camera video in Website on PC

     [raspberry pi wifi address]:8080/?action=stream
     
 <br/>
     
![image](https://github.com/user-attachments/assets/e0efa18b-4e01-45f3-a7ef-4f2a5a91d4e7)

<br/>

# Other way for Video Streaming

<br/>

### 6. Gstreamer Install in Raspberry Pi 

     sudo apt-get install libx264-dev   libjpeg-dev  libgstreamer-plugins-base1.0-dev libgstreamer-bad1.0-dev  gstreamer1.0-plugins-ugly gstreamer1.-=plugins-good gstreamer1.0-tools -y
     
     

### 7. Gstreamer Install in PC

     https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c
     
<br/>

#### 1) click Gstreamer download page

<br/>

![image](https://user-images.githubusercontent.com/122161666/227757701-c7aa0d86-58d8-4caf-a905-fe3fbb901a98.png)

<br/>

#### 2) click 1.22.1 runtime installer

<br/>

![image](https://user-images.githubusercontent.com/122161666/227757755-43e9f846-05b8-4b68-8e84-0242a40e582a.png)

<br/>

#### 3) Install  gstreamer1.0-msvc-x86_64-1.22.1.msi

After double click gstreamer1.0-msvc-x86_64-1.22.1.msi

you show like that

<br/>

![image](https://user-images.githubusercontent.com/122161666/227759214-d985e546-44dc-45a3-9821-a016283693c2.png)

<br/>

####  click red box

<br/>

![image](https://user-images.githubusercontent.com/122161666/227759239-c947c1a9-a4ea-4a60-abd8-d4a7b61a6ff7.png)

<br/>

#### click install

<br/>

#### 4) ste up in window's system environment variable

<br/>

![image](https://user-images.githubusercontent.com/122161666/227759937-4a20bf78-3f9b-4f59-9240-b7c2ca3eb2db.png)

<br/>

![image](https://user-images.githubusercontent.com/122161666/227760003-c09b6312-2e3e-4065-8553-6447fe8ece25.png)

<br/>

![image](https://user-images.githubusercontent.com/122161666/227760258-5c27ed57-e78b-414f-92c1-c8f141c0f966.png)

<br/>


#### 5) Test in raspberry pi 

     $ gst-launch-1.0  v4l2src  device=/dev/video0 ! video/x-raw,width=640,height=480,framerate=30/1 ! videoconvert ! jpegenc ! tcpserversink host=[raspberry pi wifi address]  port=5000
     
<br/>

#### 6) Test in PC

     > gst-launch-1.0  tcpclientsrc host=[raspberry pi wifi address] port=5000 ! jpegdec ! videoconvert ! rotate=3.14 ! autovideosink
     
<br/>

![image](https://user-images.githubusercontent.com/122161666/227761133-47d129c7-26e1-4a49-aa7f-bca149e3cfd6.png)

<br/>



