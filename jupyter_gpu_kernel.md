# Jupyter notebook tensorflow-gpu kernel
Reference \
https://www.pugetsystems.com/labs/hpc/The-Best-Way-to-Install-TensorFlow-with-GPU-Support-on-Windows-10-Without-Installing-CUDA-1187/

This is used after install of tensorflow-gpu, making it a jupyter notebook kernel and can be used for notebook.

Within conda virtual env where tensorflow-gpu was installed (with the tf_gpu environment activated).
```
conda install jupyter
conda install ipykernel
python -m ipykernel install --user --name tf_gpu --display-name "TensorFlow-GPU"   # tf_gpu is the virtual env.
```
Then open a jupyter notebook.
```
jupyter notebook
```
From the 'New' drop-down menu select the 'TensorFlow-GPU' kernel that was added, time to start writing code.
```
import tensorflow as tf
print ("TensorFlow version: " + tf.__version__)

(X_train, y_train), (X_test, y_test) = tf.keras.datasets.mnist.load_data()

X_train = X_train.reshape(60000,28,28,1).astype('float32')
X_test = X_test.reshape(10000,28,28,1).astype('float32')

X_train /= 255
X_test /= 255

n_classes = 10
y_train = tf.keras.utils.to_categorical(y_train, n_classes)
y_test = tf.keras.utils.to_categorical(y_test, n_classes)

model = tf.keras.models.Sequential()
model.add(tf.keras.layers.Conv2D(32, kernel_size=(3,3), activation='relu', input_shape=(28,28,1)) )
model.add(tf.keras.layers.Conv2D(64, kernel_size=(3,3), activation='relu'))
model.add(tf.keras.layers.MaxPooling2D(pool_size=(2,2)))
model.add(tf.keras.layers.Dropout(0.25))
model.add(tf.keras.layers.Flatten())          
model.add(tf.keras.layers.Dense(128, activation='relu'))
model.add(tf.keras.layers.Dropout(0.5))
model.add(tf.keras.layers.Dense(n_classes, activation='softmax'))

model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])

tensor_board = tf.keras.callbacks.TensorBoard('./logs/LeNet-MNIST-1')

model.fit(X_train, y_train, batch_size=128, epochs=3, verbose=1, validation_data=(X_test,y_test), callbacks=[tensor_board])

# tensorboard --logdir=./logs --host localhost --port 6006
```

          
  
