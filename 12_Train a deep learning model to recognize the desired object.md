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

<br/>

## Object Recognition from Video: Step-by-Step Deep Learning Model Creation

    import cv2 as cv
    import os
    import numpy as np
    import tensorflow as tf
    from tensorflow.keras.preprocessing.image import ImageDataGenerator
    from tensorflow.keras.models import Sequential
    from tensorflow.keras.layers import Conv2D, MaxPooling2D, Flatten, Dense, Dropout
    from sklearn.model_selection import train_test_split

    ## Step 1: Capture Video Footage
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
    
    frame_count = 0
    while True:
        ret, frame = cap.read()
        if not ret:
            print("캡쳐 실패")
            break
    
        frame = cv.rotate(frame, cv.ROTATE_180)
        video_orig.write(frame)  # Save video frame
    
        ## Step 2: Extract Frames from Video
        if frame_count % 10 == 0:  # Save every 10th frame as an image
            image_path = f'./data/frame_{frame_count}.jpg'
            cv.imwrite(image_path, frame)
    
        cv.imshow('Video', frame)
        frame_count += 1
    
        key = cv.waitKey(1)
        if key == 27:
            break
    
    cap.release()
    video_orig.release()
    cv.destroyAllWindows()
    
    ## Step 3: Label Images (Manual Step)
    # Manually label the saved images in the './data' directory using a tool like LabelImg.
    
    ## Step 4: Data Preprocessing
    image_paths = [os.path.join('./data', fname) for fname in os.listdir('./data') if fname.endswith('.jpg')]
    labels = []  # Assuming labels are collected after manual labeling
    images = []
    
    for image_path in image_paths:
        image = cv.imread(image_path)
        image = cv.resize(image, (150, 150))
        images.append(image)
        labels.append(0)  # Replace with actual label after manual labeling
    
    images = np.array(images) / 255.0  # Normalize pixel values
    labels = np.array(labels)
    
    # Split the data into training, validation, and testing sets
    X_train, X_temp, y_train, y_temp = train_test_split(images, labels, test_size=0.3, random_state=42)
    X_val, X_test, y_val, y_test = train_test_split(X_temp, y_temp, test_size=0.5, random_state=42)
    
    ## Step 5: Define and Compile a Deep Learning Model
    num_classes = len(set(labels))
    model = Sequential([
        Conv2D(32, (3, 3), activation='relu', input_shape=(150, 150, 3)),
        MaxPooling2D(pool_size=(2, 2)),
    
        Conv2D(64, (3, 3), activation='relu'),
        MaxPooling2D(pool_size=(2, 2)),
    
        Conv2D(128, (3, 3), activation='relu'),
        MaxPooling2D(pool_size=(2, 2)),
    
        Flatten(),
        Dense(512, activation='relu'),
        Dropout(0.5),
        Dense(num_classes, activation='softmax')
    ])
    
    model.compile(optimizer='adam',
                  loss='sparse_categorical_crossentropy',
                  metrics=['accuracy'])
    
    ## Step 6: Train the Model
    model.fit(
        X_train, y_train,
        epochs=20,
        validation_data=(X_val, y_val)
    )
    
    ## Step 7: Evaluate the Model
    test_loss, test_acc = model.evaluate(X_test, y_test)
    print(f'Test Accuracy: {test_acc}')
    
    ## Step 8: Save and Deploy the Model
    model.save('object_recognition_model.h5')
