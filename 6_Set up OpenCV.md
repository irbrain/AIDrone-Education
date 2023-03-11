# 1. Install python modules

     sudo apt install git build-essential cmake pkg-config 
     
     sudo apt install libjpeg-dev libtiff5-dev libjasper-dev libpng-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libxvidcore-dev libx264-dev
     
     sudo apt install  libfontconfig1-dev libcairo2-dev libgdk-pixbuf2.0-dev libpango1.0-dev libgtk2.0-dev libgtk-3-dev libatlas-base-dev gfortran libhdf5-dev libhdf5-103 python3-pyqt5 python3-dev

<br/>

    pip3 install imutils
    
    pip3 install --upgrade setuptools wheel numpy
    
<br/>

#  2. Install OpenCV
    
    pip3 install opencv-python 
    
    After install, please check the opencv verison
    
![image](https://user-images.githubusercontent.com/122161666/224481851-8fab6aa2-2839-40be-af89-61292b5279e0.png)

<br/>

# 3. Check Raspberry Camera 

![image](https://user-images.githubusercontent.com/122161666/224483414-ffb3dcab-2260-493f-8f91-f2b3053bea49.png)

### if detected =1,  Camera is connected
### if detected =0, Please check connection of camera with Raspberry Pi

<br/>

# 3. Install MJPG-Streamer in Rasbperry Pi Zero 2w 

     mkdir mjpg
     
     cd mjpg
     
     sudo apt install  libjpeg9-dev  imagemagick  libv4l-dev  subversion  python3-pil  
     
     sudo ln -s  /usr/include/linux/videodev2.h   /usr/include/linux/videodev.h
     
     git clone  https://github.com/neuralassembly/mjpg-streamer.git
     
     cd mjpg-streamer/mjpg-streamer-experimental
     
     make
     
     sudo make install     
    
<br/>  
     
     cd ~
     sudo nano mjpg.sh
     =>        export STREAMER_PATH=/home/USER ID/mjpg/mjpg-streamer/mjpg-streamer-experimental
               export LD_LIBRARY_PATH=$STREAMER_PATH
               $STREAMER_PATH/mjpg_streamer  –i   “input_raspicam.so” –o  “output_http.so –p 8091 –w $STREAMER_PATH/www”


  


     



     



      

    
    
    
