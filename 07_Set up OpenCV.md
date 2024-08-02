# 1. Install Libraries

<br/>

     sudo apt update  &&  sudo apt upgrade -y     
     sudo apt install build-essential cmake git pkg-config libjpeg-dev libtiff-dev libpng-dev libavcodec-dev libavformat-dev
                      libswscale-dev libv4l-dev  libxvidcore-dev libx264-dev  libgtk2.0-dev libgtk-3-dev imagemagick libv4l-dev
                      libatlas-base-dev gfortran python3-dev 


<br/>

# 2. Install python Libraries

    < numpy version is under 1.x >

# ![image](https://github.com/user-attachments/assets/d5cbfb13-e285-40db-a865-8984ea483158)

<br/>

# 3. Change SWAPSIZE to 2048

<br/>

       sudo dphys-swapfile swapoff
       
       sudo nano /etc/dphys-swapfile
 
                CONF_SWAPSIZE =2048 로 변경

       sudo  dphys-wapfile  setup

       sudo  dphys-wapfile  swapon
       
<br/>

# 4. Install Opencv  (Only for Python) 

     sudo apt install python3-opencv

### Setup OpenCV in Virtual Python

     sudo  find  /  -type  f  -name  "cv2*.so"

![image](https://github.com/user-attachments/assets/130dc82e-efcd-4d02-b157-5281d6fe9548)

     cp /usr/lib/python3.11/dist-packages/cv2/python-3.11/cv2*.so    ~/myvenv/lib/python3.11/site-packages/

<br/>

      sudo  find  /  -type  f  -name  "cv2*.so"

![image](https://github.com/user-attachments/assets/7814852b-9e1e-458c-8fc9-a6c509e4cc3f)

<br/>

### Check up The OpenCV's version

![image](https://github.com/user-attachments/assets/24f83ae2-74d2-4950-8dfb-87fd68dec429)

<br/>

# 5. Install OpneCV by Source  (If using  C/C++ and Python )

       mkdir opencv
   
       cd  opencv
   
       git clone https://github.com/opencv/opencv.git
   
       git clone https://github.com/opencv/opencv_contrib.git

       cd  ~/opencv/opencv

       mkdir build

       cd build

       cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=OFF -DWITH_LIBV4L=ON -D WITH_IPP=OFF -D WITH_1394=OFF -D BUILD_WITH_DEBUG_INFO=OFF -D BUILD_DOCS=OFF -D
            INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=OFF -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D ENABLE_NEON=ON -D WITH_QT=OFF -D WITH_GTK=ON -D WITH_OPENGL=ON -D 
            OPENCV_ENABLE_NONFREE=ON -D  OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules -D WITH_V4L=ON -D WITH_FFMPEG=ON -D WITH_XINE=ON -D ENABLE_PRECOMPILED_HEADERS=OFF -D BUILD_NEW_PYTHON_SUPPORT=ON -D 
            OPENCV_GENERATE_PKGCONFIG=ON -D  PYTHON3_PACKAGES_PATH=/usr/lib/python3.11/dist-packages  ../

  <br/>
  
       make –j4

       sudo make install

       sudo  ldconfig

<br/>

# 6. Check Raspberry Camera 

![image](https://user-images.githubusercontent.com/122161666/224483414-ffb3dcab-2260-493f-8f91-f2b3053bea49.png)

### if detected =1,  Camera is connected

### if detected =0, Please check connection of camera with Raspberry Pi

<br/>

# 7. Install MJPG-Streamer in Rasbperry Pi Zero 2w 

<br/>

###  Build  mjpg-streamer source  

     sudo apt update && sudo apt upgrade -y      

     git clone https://github.com/jacksonliam/mjpg-streamer.git

     cd mjpg-streamer/mjpg-streamer-experimental

     make

     sudo make install

     cd ~     
    
<br/>  
     
###  Edit mjpg-streamer.service  

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
               
###  Start  mjpg-streamer service 

    sudo systemctl daemon-reload
   
    sudo systemctl enable mjpg-streamer

    sudo systemctl start mjpg-streamer

<br/>

###  You can see the camera video in Website on PC

     [raspberry pi wifi address]:8080/?action=stream
     
 <br/>
     
![image](https://github.com/user-attachments/assets/e0efa18b-4e01-45f3-a7ef-4f2a5a91d4e7)

<br/>

# 8. Gstreamer Install in Raspberry Pi 

     sudo apt-get install libx264-dev   libjpeg-dev  libgstreamer-plugins-base1.0-dev libgstreamer-bad1.0-dev  gstreamer1.0-plugins-ugly gstreamer1.-=plugins-good gstreamer1.0-tools -y
     
     

# 9. Gstreamer Install in PC

     https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c
     
<br/>

### 1) click Gstreamer download page

<br/>

![image](https://user-images.githubusercontent.com/122161666/227757701-c7aa0d86-58d8-4caf-a905-fe3fbb901a98.png)

<br/>

### 2) click 1.22.1 runtime installer

<br/>

![image](https://user-images.githubusercontent.com/122161666/227757755-43e9f846-05b8-4b68-8e84-0242a40e582a.png)

<br/>

### 3) Install  gstreamer1.0-msvc-x86_64-1.22.1.msi

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

### 4) ste up in window's system environment variable

<br/>

![image](https://user-images.githubusercontent.com/122161666/227759937-4a20bf78-3f9b-4f59-9240-b7c2ca3eb2db.png)

<br/>

![image](https://user-images.githubusercontent.com/122161666/227760003-c09b6312-2e3e-4065-8553-6447fe8ece25.png)

<br/>

![image](https://user-images.githubusercontent.com/122161666/227760258-5c27ed57-e78b-414f-92c1-c8f141c0f966.png)

<br/>


### 5) Test in raspberry pi 

     $ gst-launch-1.0  v4l2src  device=/dev/video0 ! video/x-raw,width=640,height=480,framerate=30/1 ! videoconvert ! jpegenc ! tcpserversink host=[raspberry pi wifi address]  port=5000
     
<br/>

### 6) Test in PC

     > gst-launch-1.0  tcpclientsrc host=[raspberry pi wifi address] port=5000 ! jpegdec ! videoconvert ! rotate=3.14 ! autovideosink
     
<br/>

![image](https://user-images.githubusercontent.com/122161666/227761133-47d129c7-26e1-4a49-aa7f-bca149e3cfd6.png)

<br/>
