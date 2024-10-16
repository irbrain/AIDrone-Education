# "Steps to Create a Deep Learning Model for Object Recognition Using Video Footage"

### Introduction: The following are the sequential steps to create a deep learning model for object recognition by capturing images from a video and then using those images to train a model. This step-by-step guide will walk you through the entire process, from video processing to model training.

Steps:

### 1) Capture Video Footage:

Record the video footage of the object you want to recognize. This video should have clear shots from various angles to provide sufficient training data.

### 2) Extract Frames from Video:

Use video processing tools (e.g., OpenCV) to extract frames from the video. These frames will be saved as individual images to use as training data.
It is important to save frames periodically to cover different views and conditions of the object.

### 3) Label Images:

Manually label the extracted images with the appropriate object categories. This step is crucial to prepare a labeled dataset for supervised learning.
Tools like LabelImg can be used to make the labeling process easier.

### 4) Data Preprocessing:

Resize the images to a uniform size, normalize pixel values, and split the dataset into training, validation, and testing sets.
This preprocessing ensures that the model can learn effectively from the data.

### 5) Define and Compile a Deep Learning Model:

Define a convolutional neural network (CNN) model using deep learning frameworks like TensorFlow or PyTorch.
Compile the model by selecting an appropriate optimizer (e.g., Adam) and loss function (e.g., categorical cross-entropy).

### 6) Train the Model:

Train the model using the labeled image dataset. This involves feeding the data into the model, allowing it to learn the features of the object.
Monitor metrics like accuracy and loss to evaluate the training process.

### 7) Evaluate the Model:

Use the validation and test datasets to evaluate the performance of the trained model.
Make adjustments if necessary, such as fine-tuning hyperparameters to improve accuracy.

### 8) Save and Deploy the Model:

Save the trained model to a file for future use.
Deploy the model to recognize the desired object in real-time video footage or images.

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
            print("Fail Capture")
            break

        frame = cv.rotate(frame, cv.ROTATE_180)
        gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)

        video_orig.write(frame)  # Save the video's frame
        cv.imshow('Video', frame)
        key = cv.waitKey(1)
        if key == 27:
            break

    cap.release()
    video_orig.release() 
    cv.destroyAllWindows()


