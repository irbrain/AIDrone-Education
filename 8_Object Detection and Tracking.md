# Object Detection and Tracking 

### 1) Face detecting

         import cv2 as cv

         #cap = cv.VideoCapture(0)
         cap = cv.VideoCapture("[http://your raspberry pi WiFi address]:8091/?action=stream")

         face_detector = cv.CascadeClassifier(cv.data.haarcascades + "haarcascade_frontalface_default.xml")

         #face_cascade = cv.CascadeClassifier()
         # face_cascade.load(r"C:\Users\BKY-LG\anaconda3\envs\aidrone\Lib\site-packages\cv2\data\haarcascade_frontalface_default.xml")


         while(True):
                  ret, frame = cap.read()
                  frame = cv.rotate(frame, cv.ROTATE_180)
                  gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
                  gray = cv.equalizeHist(gray)
                  faces = face_detector.detectMultiScale(gray, 1.3, 5)

                  for(x, y, w, h) in faces:
                           cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 3, 4, 0)

                  cv.imshow('Face', frame)

                  if cv.waitKey(10) >= 0:
                           break

         cap.release()
         cv.destroyAllWindows()
         
<br/>

![image](https://user-images.githubusercontent.com/122161666/226922767-061ae3de-5ee4-4945-936f-9eb7843b9516.png)

<br/>

### 2) Contours

         import cv2 as cv

         img_color = cv.imread('F:/AIDrone/Real OpenCV/SourceCode/Ch16/16.1/test.jpg', cv.IMREAD_COLOR)
         img_gray = cv.cvtColor(img_color, cv.COLOR_BGR2GRAY)
         ret, img_binary = cv.threshold(img_gray, 150, 255, cv.THRESH_BINARY_INV)

         kernel = cv.getStructuringElement(cv.MORPH_ELLIPSE, (5, 5))
         img_binary = cv.morphologyEx(img_binary, cv.MORPH_CLOSE, kernel)

         contours, hierarchy = cv.findContours(img_binary, cv.RETR_LIST, cv.CHAIN_APPROX_SIMPLE)

         cv.drawContours(img_color, contours, 0, (0, 0, 255), 3)  
         cv.drawContours(img_color, contours, 1, (0, 255, 0), 3)  


         cv.imshow("result", img_color)
         cv.waitKey(0)

<br/>

![image](https://user-images.githubusercontent.com/122161666/227414393-8011a499-327c-4691-87b4-ad6899e3936f.png)

<br/>

### 3) Blue Color Tracking

         import cv2 as cv

         cap = cv.VideoCapture(0)
        
         if cap.isOpened() == False:
                  print("카메라를 열 수 없습니다.")
                  exit(1)

         while(True):
                  ret, img_frame = cap.read()
                  if ret == False:
                           print("캡쳐 실패")
                           break
                  
                  # change HSV space
                  img_hsv = cv.cvtColor(img_frame, cv.COLOR_BGR2HSV)

                  # find blue color's area by the value of Hue
                  lower_blue = (120-20, 70, 0)
                  upper_blue = (120+20, 255, 255)
                  img_mask = cv.inRange(img_hsv, lower_blue, upper_blue)

                  # improve binary image using morphology 
                  kernel = cv.getStructuringElement(cv.MORPH_RECT, (11, 11))
                  img_mask = cv.morphologyEx(img_mask, cv.MORPH_CLOSE, kernel)

                  # detect blue color's area from ariginal image
                  img_result = cv.bitwise_and(img_frame, img_frame, mask=img_mask)

                  # labeling
                  nlabels, labels, stats, centroids = cv.connectedComponentsWithStats(img_mask)

                  for i in range(nlabels):
                           
                           if i < 1:
                                    continue

                           # Get information about the size, center coordinates, and bounding rectangle of the region. 
                           area = stats[i, cv.CC_STAT_AREA]
                           center_x = int(centroids[i, 0])
                           center_y = int(centroids[i, 1])
                           left = stats[i, cv.CC_STAT_LEFT]
                           top = stats[i, cv.CC_STAT_TOP]
                           width = stats[i, cv.CC_STAT_WIDTH]
                           height = stats[i, cv.CC_STAT_HEIGHT]

                           # if the size of area is 10000 over 
                           # show the information of the area 
                           if area > 10000:
                                    # draw the bouding rectangle of region
                                    cv.rectangle(img_frame, (left, top), (left + width, top + height), (0, 0, 255), 3)
                                    # draw the circle of the ceter coordinates
                                    cv.circle(img_frame, (center_x, center_y), 5, (255, 0, 0), 3)
                                    # show the number of label
                                    cv.putText(img_frame, str(i), (left + 20, top + 20), cv.FONT_HERSHEY_SIMPLEX, 1, (255, 0, 0), 3)

                  cv.imshow('Color', img_frame)
                  cv.imshow('Result', img_result)

                  key = cv.waitKey(1)
                  if key == 27:
                           break


         cap.release()
         cv.destroyAllWindows()

<br/>

![image](https://user-images.githubusercontent.com/122161666/227416243-7d2e3078-cc46-447d-993b-f2d3b0aa0212.png)

<br/>

### 4)  Meanshift

         import cv2 as cv

         mouse_is_pressing = False
         start_x, start_y, end_x, end_y = -1,-1,-1,-1
         step = 0
         track_window  = None

         def mouse_callback(event,x,y,flags,param):
         global start_x, start_y, end_x, end_y
         global step, mouse_is_pressing, track_window

         if event == cv.EVENT_LBUTTONDOWN:
                  step = 1
                  mouse_is_pressing = True
                  start_x = x
                  start_y = y

         elif event == cv.EVENT_MOUSEMOVE:
                  if mouse_is_pressing:
                           end_x = x
                            end_y = y
                           step = 2

         elif event == cv.EVENT_LBUTTONUP:
                  mouse_is_pressing = False
                  end_x = x
                  end_y = y
                  step = 3


         cap = cv.VideoCapture(0)

         if cap.isOpened() == False:
                  print("카메라를 열 수 없습니다.")
                  exit(1)

         cv.namedWindow("Color")
         cv.setMouseCallback("Color", mouse_callback) 

         while True:
                  ret, img_color = cap.read()
                  if ret == False:
                           print("캡쳐 실패")
                           break;

                  if step == 1: 
                           cv.circle(img_color, (start_x, start_y), 10, (0, 255, 0), -1)

                  elif step == 2: 
                           cv.rectangle(img_color, (start_x, start_y), (end_x, end_y), (0, 255, 0), 3)

                  elif step == 3: 
                           if start_x > end_x:
                                    start_x, end_x = end_x, start_x
                                    start_y, end_y = end_y, start_x

                           track_window = (start_x, start_y, end_x-start_x, end_y-start_y)

                           img_hsv = cv.cvtColor(img_color, cv.COLOR_BGR2HSV)
                           img_ROI = img_hsv[start_y:end_y, start_x:end_x]

                           cv.imshow("ROI", img_ROI)

                           objectHistogram = cv.calcHist([img_ROI], [0], None, [180], (0, 180))

                           cv.normalize(objectHistogram, objectHistogram, alpha=0, beta=255, norm_type=cv.NORM_MINMAX)

                           step = step + 1

                  elif step == 4: 
                           img_hsv = cv.cvtColor(img_color, cv.COLOR_BGR2HSV)
                           bp = cv.calcBackProject([img_hsv], [0], objectHistogram, [0,180], 1)
                           ret, track_window = cv.meanShift(bp, track_window, ( cv.TERM_CRITERIA_EPS | cv.TERM_CRITERIA_COUNT, 10, 1 ))

                           x, y, w, h = track_window
                           cv.rectangle(img_color, (x,y), (x+w, y+h), (0, 0, 255), 2)

                           cv.imshow("Color", img_color)
 
                           if cv.waitKey(25) >= 0:
                                    break
        
        
 <br/>
 
 ![image](https://user-images.githubusercontent.com/122161666/227417701-7190f986-d030-44c6-8536-b97e20796c3a.png)
 
 <br/>
 

### 5) Camshift

         import cv2 as cv
         import numpy as np 

         mouse_is_pressing = False
         start_x, start_y, end_x, end_y = -1,-1,-1,-1
         step = 0
         track_window  = None


         def mouse_callback(event,x,y,flags,param):
                  global start_x, start_y, end_x, end_y
                  global step, mouse_is_pressing, track_window

                  if event == cv.EVENT_LBUTTONDOWN:
                           step = 1
                           mouse_is_pressing = True
                           start_x = x
                           start_y = y
 
                  elif event == cv.EVENT_MOUSEMOVE:
                           if mouse_is_pressing:
                                    end_x = x
                                    end_y = y
                                    step = 2

                  elif event == cv.EVENT_LBUTTONUP:
                           mouse_is_pressing = False
                           end_x = x
                           end_y = y
                           step = 3

         cap = cv.VideoCapture(0)

         if cap.isOpened() == False:
                  print("카메라를 열 수 없습니다.")
                  exit(1)
 
         cv.namedWindow("Color")
         cv.setMouseCallback("Color", mouse_callback) 


         while True:    
                  ret, img_color = cap.read()
                  if ret == False:
                           print("캡쳐 실패")
                           break;

                  if step == 1: 
                           cv.circle(img_color, (start_x, start_y), 10, (0, 255, 0), -1)

                  elif step == 2: 
                           cv.rectangle(img_color, (start_x, start_y), (end_x, end_y), (0, 255, 0), 3)

                  elif step == 3:  
                           if start_x > end_x:
                                    start_x, end_x = end_x, start_x
                                    start_y, end_y = end_y, start_x
 
                           track_window = (start_x, start_y, end_x-start_x, end_y-start_y)
 
                           img_hsv = cv.cvtColor(img_color, cv.COLOR_BGR2HSV)
                           img_ROI = img_hsv[start_y:end_y, start_x:end_x]

                           cv.imshow("ROI", img_ROI)

                           objectHistogram = cv.calcHist([img_ROI], [0], None, [180], (0, 180))

                           cv.normalize(objectHistogram, objectHistogram, alpha=0, beta=255, norm_type=cv.NORM_MINMAX)

                           step = step + 1

                  elif step == 4:
                           img_hsv = cv.cvtColor(img_color, cv.COLOR_BGR2HSV)
                           bp = cv.calcBackProject([img_hsv], [0], objectHistogram, [0,180], 1)

                           rotatedRect, track_window = cv.CamShift(bp, track_window, ( cv.TERM_CRITERIA_EPS | cv.TERM_CRITERIA_COUNT, 10, 1 ))

                           cv.ellipse(img_color, rotatedRect, (0, 0, 255), 2);

                           pts = cv.boxPoints(rotatedRect)
                           pts = np.int0(pts)

                           for i in range(4):
                                    cv.line(img_color, tuple(pts[i]), tuple(pts[(i + 1) % 4]), (0, 255, 0), 2)

        
                  cv.imshow("Color", img_color)
 
                  if cv.waitKey(25) >= 0:
                           break
        
  <br/>
  
  ![image](https://user-images.githubusercontent.com/122161666/227418366-8a72b484-413d-495b-9d20-e9853cc804e4.png)

  
  <br/>
  
