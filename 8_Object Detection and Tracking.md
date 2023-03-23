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

### 2) Tracking Specital Color 

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
 
    kernel = cv.getStructuringElement( cv.MORPH_RECT, ( 11, 11 ) )
    img_mask = cv.morphologyEx(img_mask, cv.MORPH_CLOSE, kernel)
    img_result = cv.bitwise_and(img_frame, img_frame, mask = img_mask)

    nlabels, labels, stats, centroids = cv.connectedComponentsWithStats(img_mask)

    for i in range(nlabels):
        if i < 1:
            continue

        area = stats[i, cv.CC_STAT_AREA]
        center_x = int(centroids[i, 0])
        center_y = int(centroids[i, 1]) 
        left = stats[i, cv.CC_STAT_LEFT]
        top = stats[i, cv.CC_STAT_TOP]
        width = stats[i, cv.CC_STAT_WIDTH]
        height = stats[i, cv.CC_STAT_HEIGHT]
 
        if area > 10000:
            cv.rectangle(img_frame, (left, top), (left + width, top + height),(0, 0, 255), 3)
            cv.circle(img_frame, (center_x, center_y), 5, (255, 0, 0), 3)
            cv.putText(img_frame, str(i), (left + 20, top + 20), cv.FONT_HERSHEY_SIMPLEX, 1, (255, 0, 0), 3)

    cv.imshow('Color', img_frame)
    cv.imshow('Result', img_result)

    key = cv.waitKey(1) 
    if key == 27:
        break

cap.release()
cv.destroyAllWindows()
