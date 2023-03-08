# Flight ZeroDrone with python code in PC

## 1. Insert bluetooth dongle in pc
      
![image](https://user-images.githubusercontent.com/122161666/223629741-4c32b1cb-78ee-418a-94cf-e088cf546f01.png)

## 2. Connect with my ZeroDrone

![image](https://user-images.githubusercontent.com/122161666/223643024-d1b56828-5bc2-415a-933f-7c38ab321c80.png)

      
## 3. intall irbrain module 
       pip install pyirbrain
       
## 4. try to excute exmaples

### 1) each motor contorl for 2 seconds

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
            
### 2) move ZeroDrone a certain distance

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
            
### 3) rotate ZeroDrone 

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
            
 ### 4) up and down ZeroDrone 

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
            
 ### 5) flight's velocity change 

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
            
 ### 6) RC ZeroDrone by keyboard 
 
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
