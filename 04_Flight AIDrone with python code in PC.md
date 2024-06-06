# Flight AIDrone with python code in PC

## 1. Connect Transmitter with PC  by USB Cable
<br/>    

![KakaoTalk_20240606_085057982](https://github.com/irbrain/AIDrone/assets/122161666/e6effc21-2827-4180-9aa8-3b1d017a3179)



## 2. How to control Transmitter
<br> 

![스크린샷 2024-06-06 134620](https://github.com/irbrain/AIDrone/assets/122161666/b644e3a0-6b22-44c1-8a6a-f3f92bee0bbd)


  
## 3. Setting up a developement environment on a PC

#### 1) install Anaconda (https://www.anaconda.com)
    
#### 2) Run anaconda prompt with administrator privileges.
<br>

![image](https://user-images.githubusercontent.com/122161666/223847583-ee36b5aa-ec12-4583-9f48-5703e987ab20.png)  


#### 3) Create a python virtual environment in the anaconda prompt
    
       conda create -n aidrone python=[version]
             
#### 4) Install AIDrone python Library in the anaconda prompt
     
       pip install pyirbrain
             
#### 5) Install Visual Studio Code (https://code.visualstudio.com)  


#### 6) Let's Programing in the Visual Studio Code

<br>
 
 


## 4. Test the exmaples in the Visual Studio Code 

#### 1) each motor contorl for 2 seconds

       from time import sleep
       from pyirbrain.zerodrone import *
       
       if __name__ == '__main__':    
       
            zerodrone = ZERODrone()
            zerodrone.Open("COM3")
            zerodrone.setOption(0)
            sleep(0.5)
            
            zerodroen.motor(0, 10)
            sleep(2)
            zerodrone.motor(1, 20)
            sleep(2)
            zerodrone.motor(2, 20)
            sleep(2)
            zerodrone.motor(3, 20)
            sleep(2)
            zerodrone.close()
            
#### 2) move AIDrone a certain distance

       from time import sleep
       from pyirbrain.zerodrone import *
       from pyirbrain.deflib import *
       
       ready = -1
       
       def receiveData(packet):
            global ready
            ready = packet[7] & 0x03
            
       if __name__ = '__main__':
       
            zerodrone = ZERODrone(recevieData)
            zerodrone.Open("COM3")   # change to the your ports number
            zerodrone.setOption(0)
            sleep(0.5)
            
            while ready != 0:
                  sleep(0.1)
                  
            zerodrone.takeoff()
            sleep(5)
            zerodrone.move(FRONT, 200)    # 200 means 2m
            sleep(5)
            zerodrone.move(BACK, 200) 
            sleep(5)
            zerodrone.landing()
            sleep(3)
            zerodrone.Close()
            
#### 3) rotate AIDrone 

       from time import sleep
       from pyirbrain.zerodrone import *
       from pyirbrain.deflib import *
       
       ready = -1
       
       def receiveData(packet):
            global ready
            ready = packet[7] & 0x03
            
       if __name__ = '__main__':
       
            zerodrone = ZERODrone(recevieData)
            zerodrone.Open("COM3")   # change to the your ports number
            zerodrone.setOption(0)
            sleep(0.5)
            
            while ready != 0:
                  sleep(0.1)
                  
            zerodrone.takeoff()
            sleep(5)
            zerodrone.rotation(90)    # Turn to the right about 90 degree
            sleep(5)
            zerodrone.rotation(-90) 
            sleep(5)
            zerodrone.landing()
            sleep(3)
            zerodrone.Close()
            
 #### 4) up and down AIDrone 

       from time import sleep
       from pyirbrain.zerodrone import *
       from pyirbrain.deflib import *
       
       ready = -1
       
       def receiveData(packet):
            global ready
            ready = packet[7] & 0x03
            
       if __name__ = '__main__':
       
            zerodrone = ZERODrone(recevieData)
            zerodrone.Open("COM3")   # change to the your ports number
            zerodrone.setOption(0)
            sleep(0.5)
            
            while ready != 0:
                  sleep(0.1)
                  
            zerodrone.takeoff()
            sleep(5)
            zerodrone.altitude(150)    # up to the 1.5m. basic altitude is 70cm
            sleep(5)
            zerodrone.altitude(50) 
            sleep(5)
            zerodrone.altitude(100)
            sleep(8)
            zerodrone.landing()
            sleep(3)
            zerodrone.Close()
            
 #### 5) flight's velocity change 

       from time import sleep
       from pyirbrain.zerodrone import *
       from pyirbrain.deflib import *
       
       ready = -1
       
       def receiveData(packet):
            global ready
            ready = packet[7] & 0x03
            
       if __name__ = '__main__':
       
            zerodrone = ZERODrone(recevieData)
            zerodrone.Open("COM3")   # change to the your ports number
            zerodrone.setOption(0)
            sleep(0.5)
            
            while ready != 0:
                  sleep(0.1)
                  
            zerodrone.takeoff()
            sleep(5)
            zerodrone.velocity(FRONT, 100)   
            sleep(2)
            zerodrone.velocity(FRONT, 0) 
            sleep(5)
            zerodrone.velocity(BACK, 100)
            sleep(2)
            zerodrone.velocity(BACK, 0)
            sleep(5)
            zerodrone.landing()
            sleep(5)
            zerodrone.Close()
            
 #### 6) RC AIDrone by direction keyboard 
 
      from time import sleep
      from pyirbrain.zerodrone import *
      from pyirbrain.deflib import *
      from pyirbrain.ikeyevent import *

      Height = 70
      Degree = 0

      if __name__ == '__main__':
            zerodrone = ZERODrone()
            ikey = IKeyEvent()
            zerodrone.Open("COM3")
            zerodrone.setOption(0)
            sleep(0.5)

             while not ikey.isKeyEscPressed():        
        
            if ikey.isKeyEnterPressed():             
                  zerodrone.takeoff()

            if ikey.isKeySpacePressed():
                  zerodrone.landing()
      
            if ikey.isKeyUpPressed():
                  zerodrone.velocity(FRONT, 100)
            elif ikey.isKeyDownPressed():
                  zerodrone.velocity(BACK, 100)
            else:
                  zerodrone.velocity(FRONT, 0)

            if ikey.isKeyRightPressed():
                zerodrone.velocity(RIGHT, 100)
            elif ikey.isKeyLeftPressed():
                  zerodrone.velocity(LEFT, 100)
            else:
                  zerodrone.velocity(RIGHT, 0) 
        
            if ikey.isKeyWPressed():
                  Height = Height + 10
                  zerodrone.altitude(Height)
            elif ikey.isKeyXPressed():
                  Height = Height - 10
                  zerodrone.altitude(Height)

             if ikey.isKeyDPressed():
                  Degree = Degree + 10
                  zerodrone.rotation(Degree)            
            elif ikey.isKeyAPressed():
                  Degree = Degree +10
                  zerodrone.rotation(-Degree)            

            sleep(0.1)
      zerodrone.Close()
