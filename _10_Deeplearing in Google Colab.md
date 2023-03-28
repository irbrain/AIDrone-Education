# We learn DeepLearning in Google Colab

<br/>

### 1) Start Google Colab

<br/>

<img src="https://user-images.githubusercontent.com/122161666/228062485-e734d64e-6f2e-4148-b654-46a48994ed46.png"  width="640" height="480"/>

<img src="https://user-images.githubusercontent.com/122161666/228070060-11582c63-0594-4d42-91a9-e74c887b4cb5.png" width="640" height="480"/>

<img src="https://user-images.githubusercontent.com/122161666/228070089-573c69d1-c114-435c-9750-0b649753a415.png" width="640" height="480"/>

<img src="https://user-images.githubusercontent.com/122161666/228063079-289ea76d-191a-4118-a558-421f28b735cd.png"  width="640" height="480"/>

<img src="https://user-images.githubusercontent.com/122161666/228070735-79331ebf-361f-4bf0-b6ef-181d59a431bc.png"  width="1280" height="480"/>

### After install Google Colaboratory, make new Colab notebook

<img src="https://user-images.githubusercontent.com/122161666/228070976-033a4455-0808-41b5-95e6-7475571eccd7.png" width="640" height="960"/>

<br/>

 
### 2) load your google drive 

### 3) Code


    import tensorflow as tf
    import numpy as np
    import cv2

    # Load data to be used for training.
    data_dir = 'path/to/directory'
    classes = ['object1', 'object2', 'object3']      #  Name the object you want

    images = []
    labels = []

    for idx, class_name in enumerate(classes):
        path = os.path.join(data_dir, class_name)
        for img in os.listdir(path):
            img_path = os.path.join(path, img)
            img = cv2.imread(img_path)
            img = cv2.resize(img, (224, 224)) # change the size of image
            images.append(img)
            labels.append(idx)
    
    # Preprocess the data. Normalize images and convert labels to one-hot encoding form.
    images = np.array(images) / 255.0
    labels = tf.keras.utils.to_categorical(labels)
    
    # Design the model. This example uses the VGG16 model
    model = tf.keras.applications.VGG16(weights='imagenet', include_top=False, input_shape=(224, 224, 3))

    for layer in model.layers:
        layer.trainable = False

        x = tf.keras.layers.Flatten()(model.output)
        x = tf.keras.layers.Dense(256, activation='relu')(x)
        x = tf.keras.layers.Dropout(0.5)(x)
        x = tf.keras.layers.Dense(len(classes), activation='softmax')(x)

        model = tf.keras.models.Model(model.input, x)
        
    
    # train the model.
    model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
    model.fit(images, labels, epochs=10, batch_size=32, validation_split=0.2)






