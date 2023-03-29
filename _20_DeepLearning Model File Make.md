# DeepLearing Modeling File for detectiong Object

### 1) Data collection and deep learning training

To create a deep learning code that recognizes obstacles based on data captured from obstacle photographs, you will first need to collect image data that includes obstacles and use this data to train a model.

To collect image data for training the deep learning model, it is important to gather as many different types of obstacle images as possible. This can be done by searching the internet or taking pictures with a camera or smartphone. The collected images should include both images that contain obstacles and images that do not.

Once the image data has been collected, the deep learning model can be trained based on this data.

    import tensorflow as tf
    from tensorflow.keras import datasets, layers, models
    import matplotlib.pyplot as plt
    import numpy as np

    # Load Data Image
    train_images = ... # Obstacle image data to use for learning
    train_labels = ... # Image labels to use for training (obstacle / non-obstacle)
    

    # model construction
    model = models.Sequential()
    model.add(layers.Conv2D(32, (3, 3), activation='relu', input_shape=(28, 28, 1)))
    model.add(layers.MaxPooling2D((2, 2)))
    model.add(layers.Conv2D(64, (3, 3), activation='relu'))
    model.add(layers.MaxPooling2D((2, 2)))
    model.add(layers.Conv2D(64, (3, 3), activation='relu'))

    # output layer
    model.add(layers.Flatten())
    model.add(layers.Dense(64, activation='relu'))
    model.add(layers.Dense(1))

    # model compilation
    model.compile(optimizer='adam',
                  loss=tf.keras.losses.BinaryCrossentropy(from_logits=True),
                  metrics=['accuracy'])

    # model learning
    history = model.fit(train_images, train_labels, epochs=10, validation_data=(test_images, test_labels))

