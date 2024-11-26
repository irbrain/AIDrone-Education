### 1. Install Libraries

<br/>

     sudo apt update  &&  sudo apt upgrade -y     
     sudo apt install build-essential cmake git pkg-config libjpeg-dev libtiff-dev libpng-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev  libxvidcore-dev libx264-dev  libgtk2.0-dev libgtk-3-dev imagemagick libv4l-dev libatlas-base-dev gfortran python3-dev libopenblas-dev imagemagick subversion python3-pil

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
       

### 3. Install Opencv  (Simple waw for C++ and Python) : Recommend this method

     sudo apt install libopencv-dev

     sudo apt install python3-opencv

#### Move the opencv executable to the python virtual environment

     sudo  find  /  -type  f  -name  "cv2*.so"

![image](https://github.com/user-attachments/assets/6113320f-6d1f-4a84-9098-6b6e8be3ce14)

     cp /usr/lib/python3/dist-packages/cv2.cpython-39-arm-linux-gnueabihf.so    ~/myvenv/lib/python3.9/site-packages/

#### Check up The OpenCV's version

![image](https://github.com/user-attachments/assets/473ac68a-f880-4d9e-a3f6-503a26e5823e)

<br/>

### 4. Install OpneCV by Source  (If using  C/C++ and Python )

       git clone https://github.com/opencv/opencv.git
   
       git clone https://github.com/opencv/opencv_contrib.git

       cd  ~/opencv

       mkdir build

       cd build

       cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=OFF -DWITH_LIBV4L=ON -D WITH_IPP=OFF -D WITH_1394=OFF -D BUILD_WITH_DEBUG_INFO=OFF -D BUILD_DOCS=OFF -D  INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=OFF -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D ENABLE_NEON=ON -D WITH_QT=OFF -D WITH_GTK=ON -D WITH_OPENGL=ON -D  OPENCV_ENABLE_NONFREE=ON -D  OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules -D WITH_V4L=ON -D WITH_FFMPEG=ON -D WITH_XINE=ON -D ENABLE_PRECOMPILED_HEADERS=OFF -D BUILD_NEW_PYTHON_SUPPORT=ON -D  OPENCV_GENERATE_PKGCONFIG=ON -D  PYTHON3_PACKAGES_PATH=/usr/lib/python3.9/dist-packages  ../
 
#### you have to change PYTHON3_PACKAGES_PATH's python version to your python version.

<br/>

       make –j4

       sudo make install

       sudo  ldconfig

<br/>

### 5. Install MJPG-Streamer in Rasbperry Pi Zero 2w ( Bullseye OS Version)

<br/>

#### 1) Build  mjpg-streamer source  

     sudo apt update && sudo apt upgrade -y

     git clone  https://github.com/jacksonliam/mjpg-streamer.git

     cd mjpg-streamer/mjpg-streamer-experimental

     make CMAKE_BUILD_TYPE=Debug

     sudo make install 
     
     cd ~     
    
<br/>  

#### 3) MJPG-Streamer Test

      cd mjpg-streamer/mjpg-streamer-experimental

      ./mjpg_streamer -i "input_uvc.so" -o "output_http.so -w ./www"


##### Wep page in PC 

      https://<raspberry pi Address>:8080

![image](https://github.com/user-attachments/assets/fd233dcf-a3a0-400d-9f30-4f9a15ec14f6)

<br/>

 # 6. Make Raspberry Pi automatically become MJPG STREAMER when it starts 

#### Change  www's index.html to  stream_simple.html 

      cd mjpg-streamer/mjpg-streamer-experimental/www
      
      mv index.html  index.html_backup
      
      nano  index.html 
      

![image](https://github.com/user-attachments/assets/c3c19b79-f056-4f1f-87b0-a5aa60577309)

##### Save index.html 

<br/>      
 
####  Edit mjpg-streamer.service  

     sudo nano /etc/systemd/system/mjpg-streamer.service
 
     < change from user id to your id >
    
<br/> 

     [Unit]
     
     Description=mjpg-streamer     
     After=network-online.target

     [Service]
     
     ExecStart=/home/user id/mjpg-streamer/mjpg-streamer-experimental/mjpg_streamer -i "/home/user id/mjpg-streamer/mjpg-streamer-experimental/input_uvc.so" -o 
       "/home/user id/mjpg-streamer/mjpg-streamer-experimental/output_http.so -w /home/user id/mjpg-streamer/mjpg-streamer-experimental/www -p 80"
     
     Restart=always     
     User=root

     [Install]
     
     WantedBy=multi-user.target

<br/>
               
####  Start  mjpg-streamer service 
<br/>

   sudo systemctl daemon-reload
   
   sudo systemctl enable mjpg-streamer
   
   sudo systemctl start mjpg-streamer
   
   sudo reboot

<br/>

####  You can see the camera video in Website on PC

     http://<raspberry pi wifi address>
     
![image](https://github.com/user-attachments/assets/e9f3a5fb-403b-48a2-8bf2-5143b5beb6e8)



