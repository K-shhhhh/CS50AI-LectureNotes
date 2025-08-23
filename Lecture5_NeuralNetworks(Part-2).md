📘 Lecture 5 – Neural Networks (Part 2)

🟦 TensorFlow
- TensorFlow is a Python library for building and training neural networks using backpropagation.
- Provides Keras API for high-level modeling.

Example: Distinguishing counterfeit notes from genuine notes
------------------------------------------------------------
import csv
import tensorflow as tf
from sklearn.model_selection import train_test_split

# Read data
with open("banknotes.csv") as f:
    reader = csv.reader(f)
    next(reader)
    data = []
    for row in reader:
        data.append({
            "evidence": [float(cell) for cell in row[:4]],
            "label": 1 if row[4] == "0" else 0
        })

# Split data
evidence = [row["evidence"] for row in data]
labels = [row["label"] for row in data]
X_training, X_testing, y_training, y_testing = train_test_split(
    evidence, labels, test_size=0.4
)

# Build model
model = tf.keras.models.Sequential()
model.add(tf.keras.layers.Dense(8, input_shape=(4,), activation="relu"))
model.add(tf.keras.layers.Dense(1, activation="sigmoid"))

# Compile and train
model.compile(optimizer="adam", loss="binary_crossentropy", metrics=["accuracy"])
model.fit(X_training, y_training, epochs=20)

# Evaluate
model.evaluate(X_testing, y_testing, verbose=2)


🟦 Computer Vision
- Computer vision: AI methods for analyzing and understanding images.
- Applications: face recognition, handwriting recognition, self-driving cars.
- Pixels: stored as RGB values (0–255).

Drawbacks of raw pixel input:
1. Ignores spatial structure (e.g., faces, shapes).
2. Very high dimensional input → expensive computation.


🟦 Image Convolution
- Convolution: apply filter (kernel) to pixels + neighbors.
- Produces transformed image emphasizing specific features.

Example: Edge Detection Kernel
kernel = [[-1, -1, -1],
          [-1,  8, -1],
          [-1, -1, -1]]

Python Implementation:
----------------------
from PIL import Image, ImageFilter
import sys

if len(sys.argv) != 2:
    sys.exit("Usage: python filter.py filename")

image = Image.open(sys.argv[1]).convert("RGB")
filtered = image.filter(ImageFilter.Kernel(
    size=(3, 3),
    kernel=[-1, -1, -1, -1, 8, -1, -1, -1, -1],
    scale=1
))
filtered.show()


🟦 Pooling
- Pooling reduces image size → faster computation, less sensitivity to shifts.
- Max-Pooling: choose the largest pixel value from each region.

Example:
Input 4x4 → divide into 2x2 regions → each becomes one pixel in smaller output.


🟦 Convolutional Neural Networks (CNNs)
- CNN = neural network with convolution + pooling layers before fully connected layers.
- Steps:
  1. Convolution (extract features)
  2. Pooling (reduce dimensions)
  3. Flattening (reshape for dense layers)
  4. Fully connected network

TensorFlow CNN Example (MNIST Handwriting Digits):
---------------------------------------------------
import tensorflow as tf

mnist = tf.keras.datasets.mnist
(x_train, y_train), (x_test, y_test) = mnist.load_data()
x_train, x_test = x_train/255.0, x_test/255.0
y_train = tf.keras.utils.to_categorical(y_train)
y_test = tf.keras.utils.to_categorical(y_test)

x_train = x_train.reshape(x_train.shape[0], 28, 28, 1)
x_test = x_test.reshape(x_test.shape[0], 28, 28, 1)

model = tf.keras.models.Sequential([
    tf.keras.layers.Conv2D(32, (3,3), activation="relu", input_shape=(28,28,1)),
    tf.keras.layers.MaxPooling2D(pool_size=(2,2)),
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dense(128, activation="relu"),
    tf.keras.layers.Dropout(0.5),
    tf.keras.layers.Dense(10, activation="softmax")
])

model.compile(optimizer="adam", loss="categorical_crossentropy", metrics=["accuracy"])
model.fit(x_train, y_train, epochs=10)
model.evaluate(x_test, y_test, verbose=2)

# Save model
import sys
if len(sys.argv) == 2:
    filename = sys.argv[1]
    model.save(filename)
    print(f"Model saved to {filename}")


🟦 Recurrent Neural Networks (RNNs)
- Feed-Forward NN: input → output (no loops).
- RNN: uses its own output as input → handles sequences.

Applications:
- Image captioning (sequence of words).
- Video analysis (sequence of frames).
- Machine translation (sequence of source words → target words).

Diagram:
- Input → Hidden State → Output
- Hidden state loops back into itself → carries "memory" of past information.
