# Record Object Video

### In order to recognize a desired object using deep learning, the object must be trained first. To do this, you need to prepare a labeled dataset and use it to train a model.
### The following steps are required to perform object recognition in video using the trained model.

<br/>

### 1) Load the trained model
### 2) Open video file
### 3) traversing video frames
### 4) Recognize objects in frames
### 5) Drawing labels on frames using object recognition results
### 6) Save labeled frames as video

<br/>

## 1. MPEG Streaming in ZeroDrone

    sh mjpg.sh
    
<br/>

## 2. Record Video in PC 

import cv2 as cv
import os

SCREEN_WIDTH = 320
SCREEN_HEIGHT = 240

cap = cv.VideoCapture("http://[your wifi number]:8091/?action=stream")
#cap = cv.VideoCapture(0)
cap.set(3, int(SCREEN_WIDTH))
cap.set(4, int(SCREEN_HEIGHT))

fourcc = cv.VideoWriter_fourcc(*'XVID')

try:
    if not os.path.exists('./data'):
        os.makedirs('./data')
except OSError:
    pass

video_orig = cv.VideoWriter('./data/object_video.avi', fourcc, 20.0, (SCREEN_WIDTH, SCREEN_HEIGHT))

while True:
    ret, frame = cap.read()
    if not ret:
        print("캡쳐 실패")
        break

    frame = cv.rotate(frame, cv.ROTATE_180)
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)

    video_orig.write(frame)  # 비디오 프레임 저장

    cv.imshow('Video', frame)

    key = cv.waitKey(1)
    if key == 27:
        break

cap.release()
video_orig.release()  # 비디오 파일 저장 종료
cv.destroyAllWindows()


