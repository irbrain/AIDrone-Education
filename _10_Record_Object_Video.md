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

## 2. PC Code

    import cv2 as cv

    SCREEN_WIDTH = 320
    SCREEN_HEIGHT = 240

    cap = cv.VideoCapture("http://192.168.2.5:8091/?action=stream")
    #cap = cv.VideoCapture(0)
    cap.set(3, int(SCREEN_WIDTH))
    cap.set(4, int(SCREEN_HEIGHT))

    fourcc =  cv2.VideoWriter_fourcc(*'XVID')

    try:
	    if not os.path.exists('./data'):
		    os.makedirs('./data')
    except OSError:
		pass

    video_orig = cv2.VideoWriter('./data/car_video.avi', fourcc, 20.0, (SCREEN_WIDTH, SCREEN_HEIGHT))

    while(True):
        ret, frame = cap.read()
        frame = cv.rotate(frame, cv.ROTATE_180)
    
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

        if ret == False:
            print("캡쳐 실패")
            break;  

        cv.imshow('Video', frame)

        key = cv.waitKey(1)
        if key == 27:
            break;

    cap.release()
    cv.destroyAllWindows()


