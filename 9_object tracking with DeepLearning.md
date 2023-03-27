# Control ZeroDrone by Direction Picture Using Teachable Machine

### 1)

    import cv2
    import keras
    import numpy as np
    import requests
    from bs4 import BeautifulSoup
    import threading
    from time import sleep
    from pyirbrain.zerodrone import *
    from pyirbrain.deflib import *
    from pyirbrain.ikeyevent import *


    def receiveData(packet):
        global ready
        ready = packet[7] & 0x03


    # Preprocessing of Image
    def preprocessing(frame):
        # Resizing 
        size = (224, 224)
        frame_resized = cv2.resize(frame, size, interpolation=cv2.INTER_AREA)

        # image normalization
        frame_normalized = (frame_resized.astype(np.float32) / 127.0) - 1

        # Reshape image dimensions - reshapes them for prediction.
        frame_reshaped = frame_normalized.reshape((1, 224, 224, 3))

        return frame_reshaped

    # Load the trained model
    model_filename = 'E:\AIDrone\converted_keras\keras_model.h5'
    model = keras.models.load_model(model_filename)

    # camera capture object, 0=built-in camera
    #capture = cv2.VideoCapture("http://192.168.137.142:8091/?action=stream")  # ipcam사용시 여기의 0을 바꿔주세요
    capture = cv2.VideoCapture(0)

    # 캡쳐 프레임 사이즈 조절
    capture.set(cv2.CAP_PROP_FRAME_WIDTH, 160)
    capture.set(cv2.CAP_PROP_FRAME_HEIGHT, 120)

    zerodrone = ZERODrone()
    ikey = IKeyEvent()
    zerodrone.Open("COM3")
    zerodrone.setOption(0)
    sleep(0.5)


    while True:
        ret, frame = capture.read()
        frame_fliped = cv2.flip(frame, 1)

        if cv2.waitKey(200) > 0:
            break

        preprocessed = preprocessing(frame_fliped)
        prediction = model.predict(preprocessed)


        # 학생들이 각자 만든 티쳐블 머신으로 수정한다.
        if prediction[0,0] < prediction[0,1] and prediction[0,2] < prediction[0,1] and prediction[0,3] < prediction[0,1] and prediction[0,4] < prediction[0,1]:
            cv2.putText(frame_fliped, 'takeoff', (10, 20), cv2.FONT_HERSHEY_PLAIN, 1, (0, 0, 0))
            print("takeoff")
            zerodrone.takeoff()

        elif prediction[0,0] < prediction[0,2] and prediction[0,1] < prediction[0,2] and prediction[0,3] < prediction[0,2] and prediction[0,4] < prediction[0,2]: 
            cv2.putText(frame_fliped, 'landing', (10, 20), cv2.FONT_HERSHEY_PLAIN, 1, (0, 0, 0))
            print("landing")
            zerodrone.landing()  

        elif prediction[0,0] < prediction[0,3] and prediction[0,1] < prediction[0,3] and prediction[0,2] < prediction[0,3] and prediction[0,4] < prediction[0,3]:
            cv2.putText(frame_fliped, 'Up', (10, 20), cv2.FONT_HERSHEY_PLAIN, 1, (0, 0, 0))
            print("Up")
            zerodrone.altitude(100)
    
        elif prediction[0,0] < prediction[0,4] and prediction[0,1] < prediction[0,4] and prediction[0,2] < prediction[0,4] and prediction[0,3] < prediction[0,4]:
            cv2.putText(frame_fliped, 'down', (10, 20), cv2.FONT_HERSHEY_PLAIN, 1, (0, 0, 0))
            print("down")
            zerodrone.landing()
        
        cv2.imshow("VideoFrame", frame_fliped)

        zerodrone.landing()
        capture.release()
        cv2.destroyAllWindows()
