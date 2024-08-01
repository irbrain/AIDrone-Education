# 1. Install python modules

     sudo apt update  &&  sudo apt upgrade -y     
     sudo apt install build-essential cmake git pkg-config libjpeg-dev libtiff-dev libpng-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev  libxvidcore-dev libx264-dev  libgtk2.0-dev libgtk-3-dev 
                      libatlas-base-dev gfortran python3-dev


<br/>

# 2. Change SWAPSIZE to 2048


       sudo dphys-swapfile swapoff
       
       sudo nano /etc/dphys-swapfile
 
                CONF_SWAPSIZE =2048 로 변경

       sudo  dphys-wapfile  setup

       sudo  dphys-wapfile  swapon
       
<br/>

#  3. Install OpenCV
    
    pip3 install opencv-python==4.7.0.68 
    
    After install, please check the opencv verison
    
   ![image](https://user-images.githubusercontent.com/122161666/224481851-8fab6aa2-2839-40be-af89-61292b5279e0.png)

<br/>

# 3. Install Opnecv by Source

   mkdir opencv
   
   cd  opencv
   
   git clone https://github.com/opencv/opencv.git
   
   git clone https://github.com/opencv/opencv_contrib.git


<br/>

# 4. Check Raspberry Camera 

![image](https://user-images.githubusercontent.com/122161666/224483414-ffb3dcab-2260-493f-8f91-f2b3053bea49.png)

### if detected =1,  Camera is connected
### if detected =0, Please check connection of camera with Raspberry Pi

<br/>

# 5. Install MJPG-Streamer in Rasbperry Pi Zero 2w 

     mkdir mjpg
     
     cd mjpg
     
     sudo apt install  libjpeg9-dev  imagemagick  libv4l-dev  subversion  python3-pil  
     
     sudo ln -s  /usr/include/linux/videodev2.h   /usr/include/linux/videodev.h
     
     git clone  https://github.com/neuralassembly/mjpg-streamer.git
     
     cd mjpg-streamer/mjpg-streamer-experimental
     
     make
     
     sudo make install     
    
<br/>  
     
###  make mjpg.sh file 

     cd ~
     sudo nano mjpg.sh
     =>        export STREAMER_PATH=/home/USER ID/mjpg/mjpg-streamer/mjpg-streamer-experimental
               export LD_LIBRARY_PATH=$STREAMER_PATH
               $STREAMER_PATH/mjpg_streamer  –i -rot 180  “input_raspicam.so” –o  “output_http.so –p 8091 –w $STREAMER_PATH/www”
               
     ctrl + x -> input y and then click Enter (save and out)
               
###  execute mjpg.sh

![image](https://user-images.githubusercontent.com/122161666/224519901-50eafb7c-33ef-4070-99dd-613f0caa07f9.png)


###  You can see the camera video in Website on PC

     [raspberry pi wifi address]:8091/?action=stream
     
 <br/>
     
![image](https://user-images.githubusercontent.com/122161666/224520158-66f75c2a-dabd-4cda-baf2-562c72ba2f13.png)

![image](https://user-images.githubusercontent.com/122161666/224520091-6a490660-a6e5-4fd0-883b-810778ed617d.png)

<br/>

# 4. Gstreamer Install in Raspberry Pi 

     sudo apt-get install libx264-dev   libjpeg-dev  libgstreamer-plugins-base1.0-dev libgstreamer-bad1.0-dev  gstreamer1.0-plugins-ugly gstreamer1.-=plugins-good gstreamer1.0-tools -y
     
     

# 5. Gstreamer Install in PC

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
