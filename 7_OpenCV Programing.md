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

### 2) One Template Matching
         import cv2 as cv
         import numpy as np

         img_rgb = cv.imread('F:/AIDrone/Real OpenCV/SourceCode/Ch14/test.jpg')
         img_gray = cv.cvtColor(img_rgb, cv.COLOR_BGR2GRAY)

         img_template = cv.imread('F:/AIDrone/Real OpenCV/SourceCode/Ch14/template.jpg', cv.IMREAD_GRAYSCALE)
         w, h = img_template.shape[:2]

         res = cv.matchTemplate(img_gray, img_template, cv.TM_CCOEFF_NORMED)
         min_val, max_val, min_loc, max_loc = cv.minMaxLoc(res)

         top_left = max_loc
         bottom_right = (top_left[0]+w, top_left[1]+h)


         cv.rectangle(img_rgb, top_left, bottom_right, (0, 0, 255), 2)

         cv.imshow('result', img_rgb)
         cv.waitKey(0)

<br/>

![image](https://user-images.githubusercontent.com/122161666/227339946-fed4f4b2-f04a-4e15-a8d1-c34f52d633a7.png)

<br/>

### 3) Multi Template Matching

         import cv2 as cv
         import numpy as np

         def notInList(newObject):
                  for detectedObject in detectedObjects:
                           a = newObject[0]-detectedObject[0]
                           b = newObject[1]-detectedObject[1]
                           if np.sqrt(a*a+b*b) < 5:
                                    return False
                  return True


         detectedObjects=[]

         img_rgb = cv.imread('F:/AIDrone/Real OpenCV/SourceCode/Ch14/test.jpg')
         img_gray = cv.cvtColor(img_rgb, cv.COLOR_BGR2GRAY)
         
         img_template = cv.imread('F:/AIDrone/Real OpenCV/SourceCode/Ch14/template.jpg', cv.IMREAD_GRAYSCALE)
         w, h = img_template.shape[:2]
         
         res = cv.matchTemplate(img_gray, img_template, cv.TM_CCOEFF_NORMED)

         count = 0
         for x in range(res.shape[1]):
                  for y in range(res.shape[0]):
                           if res[y, x] > 0.9 and notInList((x, y)):
                                    detectedObjects.append((x, y))
                                    cv.rectangle(img_rgb, (x, y), (x+w, y+h), [0, 0, 255], 1)
                                    count = count + 1

         print(count)
         cv.imshow('result', img_rgb)
         cv.waitKey(0)
    
<br/>

![image](https://user-images.githubusercontent.com/122161666/227341410-6bd189aa-1f0e-4f4d-b148-3b96613ddee4.png)

<br/>

### 4) Image Segmentation

#### It is used as a preprocessing process for object recognition and tracking.

#### a. Binary

         import cv2

         def nothing(x):
                  pass

         cv2.namedWindow('Binary')
         cv2.createTrackbar('threshold', 'Binary', 0, 255, nothing)
         cv2.setTrackbarPos('threshold', 'Binary', 127)

         img_color = cv2.imread('F:/AIDrone/Real OpenCV/SourceCode/Ch15/15.1/ball.jpg', cv2.IMREAD_COLOR)
         img_gray = cv2.cvtColor(img_color, cv2.COLOR_BGR2GRAY)

         while(True):
                  thre = cv2.getTrackbarPos('threshold', 'Binary')

                  ret,img_binary = cv2.threshold(img_gray, thre, 255, 
                  cv2.THRESH_BINARY_INV)

                  img_result = cv2.bitwise_and(img_color, img_color, mask = img_binary)
    
                   cv2.imshow('Result', img_result)
                   cv2.imshow('Binary', img_binary)
                  
                  if cv2.waitKey(1) == 27:
                           break


         cv2.destroyAllWindows()
                  
<br/>

![image](https://user-images.githubusercontent.com/122161666/227366562-cb654066-961c-4822-aab8-2f013d4d031e.png)

<br/>

#### b. Detecting Blue Color

         import cv2 as cv

         cap = cv.VideoCapture(0)
         if cap.isOpened() == False:
                  print("카메라를 열 수 없습니다.")
                  exit(1)

         while(True):
                  ret, img_frame = cap.read()
                  if ret == False:
                           print("캡쳐 실패")
                           break;

                  img_hsv = cv.cvtColor(img_frame, cv.COLOR_BGR2HSV)

                  lower_blue = (120-20, 70, 0)
                  upper_blue = (120+20, 255, 255)
                  img_mask = cv.inRange(img_hsv, lower_blue, upper_blue)

                  img_result = cv.bitwise_and(img_frame, img_frame, mask = img_mask)

                  cv.imshow('Color', img_frame)
                  cv.imshow('Result', img_result)

                  key = cv.waitKey(1) 
                  if key == 27:
                           break

         cap.release()
         cv.destroyAllWindows()

<br/>

![image](https://user-images.githubusercontent.com/122161666/227366400-8f0eba65-c1b7-45f2-b074-6f1f6f3a4d32.png)

<br/>

## If you want to learn OpenCV,  please refer to the link below to study
https://opencv-python.readthedocs.io/en/latest/index.html
