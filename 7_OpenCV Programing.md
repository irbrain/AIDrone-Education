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


## If you want to learn OpenCV,  please refer to the link below to study
https://opencv-python.readthedocs.io/en/latest/index.html
