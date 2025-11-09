# ===============================
# 🔹 Importación de librerías necesarias
# ===============================
import tensorflow as tf  # Librería principal de IA y aprendizaje profundo
from tensorflow.keras.datasets import mnist  # Base de datos MNIST (imágenes de dígitos 0-9)
from tensorflow.keras.models import Sequential  # Permite crear modelos de capas secuenciales
from tensorflow.keras.layers import Dense, Flatten, Dropout  # Tipos de capas neuronales usadas
from tensorflow.keras.utils import to_categorical  # Convierte etiquetas numéricas a formato categórico (one-hot)
import matplotlib.pyplot as plt  # Librería para graficar resultados del entrenamiento
import os  # Módulo para trabajar con archivos y rutas
import tkinter as tk  # Librería para interfaz gráfica básica
from tkinter import ttk, messagebox  # Widgets con estilos mejorados y cuadros de mensaje
import threading  # 👈 Permite ejecutar tareas sin congelar la interfaz (entrenamiento en segundo plano)

# ===============================
# 🔹 Función: Crear modelo MLP (red neuronal multicapa)
# ===============================
def crear_modelo():
    model = Sequential([  # Crea una secuencia de capas
        Flatten(input_shape=(28, 28)),  # Convierte cada imagen 28x28 en un vector de 784 elementos
        Dense(256, activation='relu'),  # Capa oculta con 256 neuronas y función de activación ReLU
        Dropout(0.3),  # Apaga aleatoriamente el 30% de las neuronas durante el entrenamiento (reduce sobreajuste)
        Dense(128, activation='relu'),  # Segunda capa oculta con 128 neuronas ReLU
        Dense(10, activation='softmax')  # Capa de salida con 10 neuronas (una por cada dígito 0–9)
    ])
    # Configura el modelo con optimizador, función de pérdida y métrica
    model.compile(optimizer='adam',
                  loss='categorical_crossentropy',
                  metrics=['accuracy'])
    return model  # Devuelve el modelo ya listo para entrenar


# ===============================
# 🔹 Cargar y preparar los datos de MNIST
# ===============================
(x_train, y_train), (x_test, y_test) = mnist.load_data()  # Carga el dataset en entrenamiento y prueba
x_train, x_test = x_train / 255.0, x_test / 255.0  # Normaliza los píxeles (0-255 → 0-1)
y_train, y_test = to_categorical(y_train), to_categorical(y_test)  # Convierte etiquetas a vectores binarios

# ===============================
# 🔹 Configuración de la interfaz gráfica principal
# ===============================
root = tk.Tk()  # Crea la ventana principal
root.title("Clasificador MNIST - Interfaz Visual")  # Título de la ventana
root.geometry("850x520")  # Tamaño de la ventana
root.configure(bg="#ffffff")  # Fondo blanco
root.resizable(False, False)  # No permite cambiar tamaño de la ventana

# Colores personalizados
azul = "#4B5CF0"
blanco = "#ffffff"

# Marco izquierdo decorativo (zona visual de título)
left_frame = tk.Frame(root, bg=azul, width=300, height=520)
left_frame.pack(side="left", fill="y")  # Alinea al lado izquierdo y llena en vertical

# Título principal dentro del marco izquierdo
titulo = tk.Label(left_frame, text="🧠 Clasificador\nMNIST",
                  font=("Poppins", 22, "bold"), bg=azul, fg="white", justify="center")
titulo.place(relx=0.5, rely=0.3, anchor="center")  # Centra el texto vertical y horizontalmente

# Subtítulo decorativo
subtitulo = tk.Label(left_frame, text="Red Neuronal MLP",
                     font=("Poppins", 13), bg=azul, fg="white")
subtitulo.place(relx=0.5, rely=0.45, anchor="center")

# Marco derecho (zona de control con botones)
right_frame = tk.Frame(root, bg=blanco, width=550, height=520)
right_frame.pack(side="right", fill="both", expand=True)

# ===============================
# 🔹 Variables globales
# ===============================
model = None  # Almacena el modelo entrenado
history = None  # Guarda el historial del entrenamiento (precisión, pérdida, etc.)

# ===============================
# 🔹 Ventana de progreso del entrenamiento
# ===============================
def mostrar_progreso():
    # Crea una ventana secundaria mientras se entrena el modelo
    progreso = tk.Toplevel(root)
    progreso.title("Entrenamiento en Progreso")
    progreso.geometry("600x400")
    progreso.config(bg="#f7f8ff")

    # Etiqueta informativa en la parte superior
    label = tk.Label(progreso, text="⏳ Entrenando la red neuronal...", 
                     font=("Poppins", 14, "bold"), bg="#f7f8ff", fg=azul)
    label.pack(pady=10)

    # Cuadro de texto para mostrar la salida de la consola (logs del entrenamiento)
    text_area = tk.Text(progreso, height=18, width=70, bg="black", fg="lime", insertbackground="lime")
    text_area.pack(padx=10, pady=10)

    # Clase interna para redirigir los prints de consola hacia el cuadro de texto
    class ConsoleRedirect:
        def __init__(self, widget):
            self.widget = widget

        def write(self, text):
            self.widget.insert(tk.END, text)  # Inserta texto al final
            self.widget.see(tk.END)  # Desplaza hacia el final automáticamente
            self.widget.update_idletasks()  # Actualiza interfaz

        def flush(self):
            pass  # Requerido para compatibilidad con sys.stdout

    import sys
    sys.stdout = ConsoleRedirect(text_area)  # 👈 Redirige todos los print() a la ventana de progreso

    return progreso  # Retorna la ventana para poder cerrarla luego


# ===============================
# 🔹 Funciones que ejecutan las acciones de los botones
# ===============================
def entrenar_modelo():
    # Función anidada que realiza el entrenamiento real (en un hilo aparte)
    def ejecutar_entrenamiento():
        global model, history
        progreso = mostrar_progreso()  # Abre la ventana de progreso
        model = crear_modelo()  # Crea un nuevo modelo MLP

        print("\n===== ENTRENANDO MODELO =====\n")
        history = model.fit(  # Inicia el entrenamiento
            x_train, y_train,
            epochs=10,  # Número de pasadas por todo el dataset
            batch_size=128,  # Tamaño del grupo de datos procesados por iteración
            validation_split=0.1,  # Usa el 10% de los datos para validación
            verbose=2  # Muestra detalles del entrenamiento (épocas)
        )

        print("\n✅ Entrenamiento finalizado correctamente.")
        progreso.destroy()  # Cierra la ventana de progreso

        # Muestra un gráfico con la precisión del modelo
        plt.figure()
        plt.plot(history.history['accuracy'], label='Entrenamiento')
        plt.plot(history.history['val_accuracy'], label='Validación')
        plt.title('Precisión del Modelo')
        plt.xlabel('Épocas')
        plt.ylabel('Precisión')
        plt.legend()
        plt.show()

    # 👇 Crea un hilo para no congelar la interfaz durante el entrenamiento
    threading.Thread(target=ejecutar_entrenamiento).start()


def evaluar_modelo():
    global model
    # Evalúa el modelo ya entrenado con los datos de prueba
    if model:
        test_loss, test_acc = model.evaluate(x_test, y_test, verbose=0)
        messagebox.showinfo("Evaluación", f"✅ Precisión en prueba: {test_acc:.4f}")
    else:
        messagebox.showwarning("Atención", "Primero debes entrenar o cargar un modelo.")


def guardar_modelo():
    global model
    # Guarda el modelo entrenado en un archivo .h5
    if model:
        model.save("modelo_mnist.h5")
        messagebox.showinfo("Guardar", "💾 Modelo guardado como 'modelo_mnist.h5'.")
    else:
        messagebox.showwarning("Error", "⚠️ No hay modelo para guardar.")


def cargar_modelo():
    global model
    # Carga un modelo previamente guardado si existe
    if os.path.exists("modelo_mnist.h5"):
        model = tf.keras.models.load_model("modelo_mnist.h5")
        messagebox.showinfo("Cargar", "📂 Modelo cargado correctamente.")
    else:
        messagebox.showerror("Error", "❌ No se encontró 'modelo_mnist.h5'.")


def salir():
    root.destroy()  # Cierra la aplicación


# ===============================
# 🔹 Creación de los botones y texto del panel derecho
# ===============================
lbl_titulo = tk.Label(right_frame, text="Panel de Control",
                      font=("Poppins", 20, "bold"), bg=blanco, fg=azul)
lbl_titulo.pack(pady=30)

# Botón para iniciar el entrenamiento
btn_entrenar = ttk.Button(right_frame, text="Entrenar Modelo", command=entrenar_modelo)
btn_entrenar.pack(pady=10, ipadx=20, ipady=8)

# Botón para evaluar el modelo entrenado
btn_evaluar = ttk.Button(right_frame, text="Evaluar Modelo", command=evaluar_modelo)
btn_evaluar.pack(pady=10, ipadx=20, ipady=8)

# Botón para guardar el modelo en archivo
btn_guardar = ttk.Button(right_frame, text="Guardar Modelo", command=guardar_modelo)
btn_guardar.pack(pady=10, ipadx=20, ipady=8)

# Botón para cargar un modelo existente
btn_cargar = ttk.Button(right_frame, text="Cargar Modelo", command=cargar_modelo)
btn_cargar.pack(pady=10, ipadx=20, ipady=8)

# Botón para cerrar la aplicación
btn_salir = ttk.Button(right_frame, text="Salir", command=salir)
btn_salir.pack(pady=20, ipadx=20, ipady=8)

# ===============================
# 🔹 Personalización del estilo visual de botones
# ===============================
style = ttk.Style()
style.theme_use("clam")  # Usa un tema moderno tipo “clam”
style.configure("TButton",
                background=azul,
                foreground="white",
                font=("Poppins", 11, "bold"),
                borderwidth=0,
                relief="flat")
style.map("TButton",
          background=[("active", "#6C79F6")])  # Cambia color cuando se presiona

# ===============================
# 🔹 Inicia el bucle principal de la interfaz
# ===============================
root.mainloop()  # Mantiene la ventana abierta hasta que el usuario cierre