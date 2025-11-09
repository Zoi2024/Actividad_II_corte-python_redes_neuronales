azul = "#4B5CF0"
blanco = "#ffffff"

# Panel izquierdo decorativo
left_frame = tk.Frame(root, bg=azul, width=300, height=520)
left_frame.pack(side="left", fill="y")

# Título y subtítulo
titulo = tk.Label(left_frame, text="🧠 Clasificador\nMNIST",
                  font=("Poppins", 22, "bold"), bg=azul, fg="white")
titulo.place(relx=0.5, rely=0.3, anchor="center")

subtitulo = tk.Label(left_frame, text="Red Neuronal MLP",
                     font=("Poppins", 13), bg=azul, fg="white")
subtitulo.place(relx=0.5, rely=0.45, anchor="center")