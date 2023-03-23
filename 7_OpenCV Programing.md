# 1. OpemCV Programing

### 1). Show zeroDrone Video Steaming 

         import cv2 as cv

         cap = cv.VideoCapture("http:// [rasbperry pi zero 2W WIFI address]/?action=stream")
         #cap = cv.VideoCapture(0)

         while(True):
                   ret, frame = cap.read()
                   frame = cv.rotate(frame, cv.ROTATE_180)
  
                   if ret == False:
                           print("캡쳐 실패")
                           break;  
  
                  img = cv.resize(frame, (640,480))
                  cv.imshow('Video', img)
  
                  key = cv.waitKey(1)
                  if key == 27:
                           break;
  
         cap.release()
         cv.destroyAllWindows()
        
<br/>

![image](https://user-images.githubusercontent.com/122161666/226897074-4632f2d5-954d-449f-b6a1-2f26957d8a0c.png)

<br/>



## If you want to learn OpenCV,  please refer to the link below to study
https://opencv-python.readthedocs.io/en/latest/index.html
