```python
def entrenar_modelo():
    def ejecutar_entrenamiento():
        global model, history
        progreso = mostrar_progreso()
        model = crear_modelo()

        print("\n===== ENTRENANDO MODELO =====\n")
        history = model.fit(
            x_train, y_train,
            epochs=10,
            batch_size=128,
            validation_split=0.1,
            verbose=2
        )

        print("\n✅ Entrenamiento finalizado correctamente.")
        progreso.destroy()

        # Muestra el gráfico de precisión
        plt.figure()
        plt.plot(history.history['accuracy'], label='Entrenamiento')
        plt.plot(history.history['val_accuracy'], label='Validación')
        plt.title('Precisión del Modelo')
        plt.xlabel('Épocas')
        plt.ylabel('Precisión')
        plt.legend()
        plt.show()

    threading.Thread(target=ejecutar_entrenamiento).start()