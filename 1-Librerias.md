# 🧠 Clasificador MNIST con Interfaz Gráfica (Tkinter + TensorFlow)

Este proyecto combina Inteligencia Artificial con una interfaz visual en Python, utilizando `TensorFlow` para el modelo de red neuronal y `Tkinter` para la interfaz gráfica.

---

## 🔹 Descripción General

El programa entrena una red neuronal multicapa (MLP) para reconocer dígitos manuscritos del conjunto de datos MNIST, mostrando resultados mediante una interfaz interactiva.

Incluye
- Entrenamiento visual con ventana de progreso.
- Gráfico de precisión del modelo.
- Guardado y carga del modelo entrenado.
- Panel de control estético y responsivo.

---

## 🚀 Librerías Utilizadas

```python
# Librerías principales de IA y GUI
import tensorflow as tf                     # Red neuronal y aprendizaje profundo
from tensorflow.keras.datasets import mnist # Base de datos de dígitos (0–9)
from tensorflow.keras.models import Sequential  # Estructura de red secuencial
from tensorflow.keras.layers import Dense, Flatten, Dropout  # Capas neuronales
from tensorflow.keras.utils import to_categorical  # One-hot encoding
import matplotlib.pyplot as plt             # Graficar resultados
import os                                  # Manejo de archivos
import tkinter as tk                       # Interfaz gráfica
from tkinter import ttk, messagebox        # Estilos y mensajes
import threading                           # Procesos paralelos (no bloquear GUI)