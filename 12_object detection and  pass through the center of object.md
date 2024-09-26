#  A drone tracks an object, measures distance and passes through the center of the object

<br/>

### To implement this, various functionalities such as drone movement, object tracking, and distance measurement are needed. Here are some examples of necessary functionalities:

#### 1. Drone control: Use an API or library to control the drone. This can be used to control the drone's movement, rotation, altitude, etc.

#### 2. Object recognition: Use a computer vision library for object recognition to detect objects from the camera images.

#### 3. Object tracking: Record the position of the object and predict its movement using tracking algorithms.

#### 4. Distance measurement: Install a distance measurement sensor on the drone to measure the distance between the object and the drone.

#### 5. Passing through the center of the object: Calculate and adjust the drone's movement path to pass through the center of the object. Use the object's location and distance information for this.

## By combining these functionalities, a drone equipped with object tracking and center passing capabilities can be created. The code follows the following logical sequence:

#### 1. Drone initialization and API control setup.

#### 2. Object recognition and tracking loop setup.

#### 3. Measure distance using object and drone location.

#### 4. Calculate the drone's movement path when the distance is close enough.

#### 5. Move the drone according to the movement path.

#### 6. Repeat steps 2-5 to pass through the center of the object.

### The actual code will vary greatly depending on the API and libraries used. This will depend on what specific API and libraries will be used in the future.


## 이를 구현하기 위해서는 드론의 이동, 객체 추적 및 거리 측정 등의 다양한 기능이 필요합니다. 여기에는 필요한 기능의 예시가 있습니다.

#### 1. 드론 제어: 드론을 제어하기 위한 API 또는 라이브러리를 사용합니다. 이를 통해 드론의 이동, 회전, 고도 등을 제어할 수 있습니다.

#### 2. 객체 인식: 객체 인식을 위한 컴퓨터 비전 라이브러리를 사용하여 카메라로부터 들어오는 이미지에서 객체를 탐지합니다.

#### 3. 객체 추적: 객체를 추적하기 위해 객체의 위치를 기록하고 추적 알고리즘을 사용하여 객체의 움직임을 예측합니다.

#### 4. 거리 측정: 거리 측정을 위해 드론에 거리 측정 센서를 설치합니다. 이를 통해 객체와 드론 간의 거리를 측정할 수 있습니다.

#### 5.객체 중심부 통과: 객체 중심부를 통과하기 위해 드론의 이동 경로를 계산하고 조절합니다. 이를 위해 객체 위치와 거리 정보를 사용합니다.

## 이러한 기능들을 조합하여 객체 추적 및 중심부 통과 기능을 갖춘 드론을 만들 수 있습니다. 코드는 다음과 같은 논리적인 순서를 따릅니다.

#### 1. 드론 초기화 및 제어 API 설정

#### 2. 객체 인식 및 추적 루프 설정

#### 3. 추적된 객체의 위치와 드론의 위치를 이용하여 거리 측정

#### 4. 거리가 충분히 가까워졌을 때 드론의 이동 경로 계산

#### 5. 드론 이동 경로에 따른 드론 이동

#### 6. 2-5를 반복하며 객체 중심부 통과

## 실제 코드는 사용하는 API 및 라이브러리에 따라 매우 다를 수 있습니다. 이는 추후에 구체적으로 어떤 API 및 라이브러리를 사용할지에 따라 달라질 것입니다.

<br/>

# 1. Object Tracking Example ( Count the Viechle in the high way )

    import cv2
    from tracker import *

    # Create tracker object
    tracker = EuclideanDistTracker()

    cap = cv2.VideoCapture("F:\pysource\object_tracking_1\highway.mp4")

    # Object detection from Stable camera
    object_detector = cv2.createBackgroundSubtractorMOG2(history=100, varThreshold=40)

    while True:
        ret, frame = cap.read()
        height, width, _ = frame.shape

        # Extract Region of interest
        roi = frame[340: 720,500: 800]

        # 1. Object Detection
        mask = object_detector.apply(roi)
        _, mask = cv2.threshold(mask, 254, 255, cv2.THRESH_BINARY)
        contours, _ = cv2.findContours(mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
        detections = []
        
        for cnt in contours:
            # Calculate area and remove small elements
            area = cv2.contourArea(cnt)
            if area > 100:
                #cv2.drawContours(roi, [cnt], -1, (0, 255, 0), 2)
                x, y, w, h = cv2.boundingRect(cnt)

                detections.append([x, y, w, h])

        # 2. Object Tracking
        boxes_ids = tracker.update(detections)
        for box_id in boxes_ids:
            x, y, w, h, id = box_id
            cv2.putText(roi, str(id), (x, y - 15), cv2.FONT_HERSHEY_PLAIN, 2, (255, 0, 0), 2)
            cv2.rectangle(roi, (x, y), (x + w, y + h), (0, 255, 0), 3)

        cv2.imshow("roi", roi)
        cv2.imshow("Frame", frame)
        cv2.imshow("Mask", mask)

        key = cv2.waitKey(30)
        if key == 27:
            break

    cap.release()
    cv2.destroyAllWindows()
    
<br/>

# 2. Calculate the distance of Object and Find the center of object 













