lbl_titulo = tk.Label(right_frame, text="Panel de Control",
                      font=("Poppins", 20, "bold"), bg=blanco, fg=azul)
lbl_titulo.pack(pady=30)

btn_entrenar = ttk.Button(right_frame, text="Entrenar Modelo", command=entrenar_modelo)
btn_evaluar = ttk.Button(right_frame, text="Evaluar Modelo", command=evaluar_modelo)
btn_guardar = ttk.Button(right_frame, text="Guardar Modelo", command=guardar_modelo)
btn_cargar = ttk.Button(right_frame, text="Cargar Modelo", command=cargar_modelo)
btn_salir = ttk.Button(right_frame, text="Salir", command=root.destroy)

# Empaquetado de botones
for btn in [btn_entrenar, btn_evaluar, btn_guardar, btn_cargar, btn_salir]:
    btn.pack(pady=10, ipadx=20, ipady=8)