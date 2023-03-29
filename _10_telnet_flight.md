# Control gstreaming Zerodrone from PC

### 1) ZeroDrone Code

    import socket
    import struct
    import cv2
    import pickle


    VIDSRC = 'v4l2src device=/dev/video0 ! video/x-raw,width=160,height=120,framerate=20/1 ! videoscale ! videoconvert ! jpegenc ! appsink'
    cap=cv2.VideoCapture(VIDSRC, cv2.CAP_GSTREAMER)

    HOST = ''
    PORT = 8089

    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    print('Socket created')
    server.bind((HOST, PORT))
    print('Socket bind complete')
    server.listen(10)
    print('Socket now listening')
    server_cam, addr = server.accept()
    print('New Client.')

    while True:	
        # capture camera data
        ret,frame=cap.read()

        # Serialize frame
        data = pickle.dumps(frame)

        # send sensor + camera data
        data_size = struct.pack("!L", len(data)) 
        server_cam.sendall(data_size + data)


    server_cam.close()
    server.close()


### 2) Pc Code

    import socket
    import struct
    import numpy as np
    import cv2
    import pickle
    import time

    HOST_RPI = '192.168.2.5'
    PORT = 8089

    client_cam = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_cam.connect((HOST_RPI, PORT))

    t_now = time.time()
    t_prev = time.time()
    cnt_frame = 0

    while True:
        # 영상 받기
        data_len_bytes = client_cam.recv(4)
        data_len = struct.unpack('!L', data_len_bytes)

        frame_data = client_cam.recv(data_len[0], socket.MSG_WAITALL)

        # Extract frame
        frame = pickle.loads(frame_data)

        # 영상 출력
        np_data = np.frombuffer(frame, dtype='uint8')
        frame = cv2.imdecode(np_data, 1)
        frame = cv2.rotate(frame, cv2.ROTATE_180)
        frame2 = cv2.resize(frame, (320, 240))
        cv2.imshow('frame', frame2)

        key = cv2.waitKey(1)
        if key == 27:
            break

        cnt_frame += 1
        t_now = time.time()
        if t_now - t_prev >= 1.0:
            t_prev = t_now
            print("frame count : %f" % cnt_frame)
            cnt_frame = 0

    client_cam.close()




