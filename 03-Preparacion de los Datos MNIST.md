```python
(x_train, y_train), (x_test, y_test) = mnist.load_data()  # Carga el dataset
x_train, x_test = x_train / 255.0, x_test / 255.0          # Normaliza los valores
y_train, y_test = to_categorical(y_train), to_categorical(y_test)  # One-hot(x_train, y_train), (x_test, y_test) = mnist.load_data()  # Carga el dataset
x_train, x_test = x_train / 255.0, x_test / 255.0          # Normaliza los valores
y_train, y_test = to_categorical(y_train), to_categorical(y_test)  # One-hot