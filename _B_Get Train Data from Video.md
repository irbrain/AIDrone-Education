# Get Train Data From Video File 

### 1. Save the image file of obejct What you like to detect from your video file

<br/>

    import cv2
    import os
    import time

    # Create image folder if it doesn't exist
    if not os.path.exists("images"):
        os.makedirs("images")

    # Open video file
    video = cv2.VideoCapture("Video File")

    # select objects to track
    ret, frame = video. read()
    bbox = cv2.selectROI(frame, False)

    # Initialize the tracker
    tracker = cv2.TrackerCSRT_create()
    tracker.init(frame, bbox)

    # Initialize timer and counter
    timer = time.time() 
    counter = 0

    # video output
    while True:
        # read frames
        ret, frame = video. read()
        if not ret:
            break
    
        # object tracking
        success, bbox = tracker. update(frame)
    
        # output trace result
        if success:
            x, y, w, h = [int(i) for i in bbox]
            cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
        
            # Check if timer has expired and save object image
            current_time = time.time()
            if current_time - timer >= 1:    # saving image for 1 second
                counter += 1
                object_img = frame[y:y+h, x:x+w]
                img_path = os.path.join("images", f"object_img_{counter}.jpg")
                cv2.imwrite(img_path, object_img)
                timer = current_time
        else:
            cv2.putText(frame, "Failed to track object", (100, 80), cv2.FONT_HERSHEY_SIMPLEX, 0.75,(0,0,255),2)
    
        # frame output
        cv2.imshow('Object Tracking', frame)
        
        # Handling keyboard input
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    # Release the video file and close the window
    video. release()
    cv2.destroyAllWindows()
   
<br/>

#### After excute above code,  Select a ROI then press SPACE and ENTER button.  it will save the image at images folder for 1 second

<br/>

![image](https://user-images.githubusercontent.com/122161666/231761598-60b945fc-ef0b-4c80-a982-e2d667a1ca06.png)

<br/>

![image](https://user-images.githubusercontent.com/122161666/231630566-a022b437-d6fa-4eb5-8045-f1eebc9df8f7.png)




