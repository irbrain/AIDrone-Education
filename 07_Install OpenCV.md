### 1. Install Libraries

<br/>

     sudo apt update  &&  sudo apt upgrade -y     
     sudo apt install build-essential cmake git pkg-config libjpeg-dev libtiff-dev libpng-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev  libxvidcore-dev libx264-dev  libgtk2.0-dev libgtk-3-dev imagemagick libv4l-dev libatlas-base-dev gfortran python3-dev libopenblas-dev python3-pip imagemagick subversion python3-pil

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

![image](https://github.com/user-attachments/assets/83d8e0a3-f780-4bc3-95e0-5e7ba8473a99)

<br/>

### 4. Install OpneCV by Source  (If using  C/C++ and Python )

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

### 5. Install MJPG-Streamer in Rasbperry Pi Zero 2w ( Bookworm OS Version)

<br/>

#### 1) Build  mjpg-streamer source  

     sudo apt update && sudo apt upgrade -y      

     git clone https://github.com/jacksonliam/mjpg-streamer.git

     cd mjpg-streamer/mjpg-streamer-experimental

     make

     cd ~     
    
<br/>  

#### 2) The latest Raspberry Pi OS uses libcamera by default, but you can enable V4L2 compatibility mode to create a /dev/video0 device.

        sudo nano /boot/firmware/config.txt


![image](https://github.com/user-attachments/assets/f92c243c-e609-4fdb-87cd-82582f2326f3)

        sudo reboot 

<br/>

#### 3) MJPG-Streamer Test

       
       
        
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



