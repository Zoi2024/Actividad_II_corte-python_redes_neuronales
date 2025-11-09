```python
def crear_modelo():
    model = Sequential([
        Flatten(input_shape=(28, 28)),         # Convierte imágenes 28x28 en vector 784
        Dense(256, activation='relu'),         # Capa oculta con 256 neuronas
        Dropout(0.3),                          # Evita sobreajuste (dropout del 30%)
        Dense(128, activation='relu'),         # Segunda capa oculta
        Dense(10, activation='softmax')        # Capa de salida (10 dígitos)
    ])
    model.compile(optimizer='adam',
                  loss='categorical_crossentropy',
                  metrics=['accuracy'])
    return model