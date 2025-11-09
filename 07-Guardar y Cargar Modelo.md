```python
def guardar_modelo():
    if model:
        model.save("modelo_mnist.h5")
        messagebox.showinfo("Guardar", "💾 Modelo guardado como 'modelo_mnist.h5'.")
    else:
        messagebox.showwarning("Error", "⚠️ No hay modelo para guardar.")

def cargar_modelo():
    if os.path.exists("modelo_mnist.h5"):
        model = tf.keras.models.load_model("modelo_mnist.h5")
        messagebox.showinfo("Cargar", "📂 Modelo cargado correctamente.")
    else:
        messagebox.showerror("Error", "❌ No se encontró 'modelo_mnist.h5'.")