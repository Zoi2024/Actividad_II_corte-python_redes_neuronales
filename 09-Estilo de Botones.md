```python

style = ttk.Style()
style.theme_use("clam")
style.configure("TButton",
                background=azul,
                foreground="white",
                font=("Poppins", 11, "bold"),
                borderwidth=0,
                relief="flat")
style.map("TButton",
          background=[("active", "#6C79F6")])