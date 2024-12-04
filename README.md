import tkinter as tk
from tkinter import messagebox

# Lista para almacenar las ventas
registro_ventas = []

# Función para calcular el total de la venta actual
def calcular_total():
    try:
        cantidad = int(entry_cantidad.get())
        precio_unitario = float(entry_precio.get())
        total = cantidad * precio_unitario
        label_total.config(text=f"Total: ${total:.2f}")
    except ValueError:
        messagebox.showerror("Error", "Por favor, ingresa valores válidos.")

# Función para agregar la venta al registro
def registrar_venta():
    try:
        tipo_jabon = entry_tipo.get()
        cantidad = int(entry_cantidad.get())
        precio_unitario = float(entry_precio.get())
        total = cantidad * precio_unitario

        if not tipo_jabon.strip():
            raise ValueError("El tipo de jabón no puede estar vacío.")
        
        # Registrar la venta 
        venta = {
            "Tipo de Jabon": tipo_jabon,
            "Cantidad": cantidad,
            "Precio Unitario": precio_unitario,
            "Total": total
        }
        registro_ventas.append(venta)

        # Mostrar mensaje y limpiar campos
        messagebox.showinfo("Venta Registrada", f"Venta de {cantidad} x '{tipo_jabon}' registrada con éxito.\nTotal: ${total:.2f}")
        limpiar_campos()
        actualizar_registro()
    except ValueError as e:
        messagebox.showerror("Error", str(e))

# Función para limpiar los campos
def limpiar_campos():
    entry_tipo.delete(0, tk.END)
    entry_cantidad.delete(0, tk.END)
    entry_precio.delete(0, tk.END)
    label_total.config(text="Total: $0.00")

# Función para abrir ventana de datos fiscales
def abrir_datos_fiscales():
    ventana_datos = tk.Toplevel(ventana)
    ventana_datos.title("Datos Fiscales")
    ventana_datos.geometry("400x300")
    ventana_datos.config(bg="#d1d1d1")

    tk.Label(ventana_datos, text="Nombre:", bg="#d1d1d1").pack(pady=5)
    entry_nombre = tk.Entry(ventana_datos)
    entry_nombre.pack(pady=5)
    
    tk.Label(ventana_datos, text="RFC:", bg="#d1d1d1").pack(pady=5)
    entry_rfc = tk.Entry(ventana_datos)
    entry_rfc.pack(pady=5)
    
    tk.Label(ventana_datos, text="Dirección:", bg="#d1d1d1").pack(pady=5)
    entry_direccion = tk.Entry(ventana_datos)
    entry_direccion.pack(pady=5)
    
    tk.Label(ventana_datos, text="Correo electrónico:", bg="#d1d1d1").pack(pady=5)
    entry_correo = tk.Entry(ventana_datos)
    entry_correo.pack(pady=5)
    
    # Función para guardar los datos fiscales
    def guardar_datos_fiscales():
        nombre = entry_nombre.get().strip()
        rfc = entry_rfc.get().strip()
        direccion = entry_direccion.get().strip()
        correo = entry_correo.get().strip()

        if not nombre or not rfc or not direccion or not correo:
            messagebox.showerror("Error", "Todos los campos son obligatorios.")
            return
        
        datos_fiscales = {
            "Nombre": nombre,
            "RFC": rfc,
            "Dirección": direccion,
            "Correo Electrónico": correo
        }

        # Mostrar mensaje de éxito
        messagebox.showinfo("Datos Guardados", "Los datos fiscales se han guardado correctamente.")
        ventana_datos.destroy()  # Cerrar la ventana de datos fiscales

    # Botón para guardar los datos
    tk.Button(
        ventana_datos,
        text="Guardar Datos",
        command=guardar_datos_fiscales,
        bg="#A9DFBF"
    ).pack(pady=20)

# Función para actualizar el registro de las ventas
def actualizar_registro():
    texto_registro = "Historial de Ventas:\n\n"
    for i, venta in enumerate(registro_ventas, start=1):
        texto_registro += (
            f"{i}. {venta['Tipo de Jabon']} - {venta['Cantidad']} x ${venta['Precio Unitario']:.2f} c/u = ${venta['Total']:.2f}\n"
        )
    label_registro.config(text=texto_registro)

# Crear la ventana principal
ventana = tk.Tk()
ventana.title("Sistema de Venta de Jabones")
ventana.geometry("600x500")

# Panel de entrada de datos
frame_entrada = tk.Frame(ventana, pady=20)
frame_entrada.pack()

tk.Label(frame_entrada, text="Tipo de Jabón:").grid(row=0, column=0, padx=5, pady=5)
entry_tipo = tk.Entry(frame_entrada)
entry_tipo.grid(row=0, column=1, padx=5, pady=5) 

tk.Label(frame_entrada, text="Cantidad:").grid(row=1, column=0, padx=5, pady=5)
entry_cantidad = tk.Entry(frame_entrada)
entry_cantidad.grid(row=1, column=1, padx=5, pady=5)

tk.Label(frame_entrada, text="Precio Unitario:").grid(row=2, column=0, padx=5, pady=5)
entry_precio = tk.Entry(frame_entrada)
entry_precio.grid(row=2, column=1, padx=5, pady=5)

# Botones y resultado de cálculo
frame_botones = tk.Frame(ventana, pady=20)
frame_botones.pack()

tk.Button(frame_botones, text="Calcular Total", command=calcular_total, bg="#A9DFBF").grid(row=0, column=0, padx=10)
tk.Button(frame_botones, text="Registrar Venta", command=registrar_venta, bg="#A9DFBF").grid(row=0, column=1, padx=10)
tk.Button(frame_botones, text="Limpiar", command=limpiar_campos, bg="#F1948A").grid(row=0, column=2, padx=10)
tk.Button(frame_botones, text="Datos Fiscales", command=abrir_datos_fiscales, bg="#4169E1").grid(row=0, column=3, padx=10)

label_total = tk.Label(ventana, text="Total: $0.00", font=("Arial", 14))
label_total.pack(pady=10)

# Registro de ventas
frame_registro = tk.Frame(ventana, pady=20)
frame_registro.pack()

label_registro = tk.Label(frame_registro, text="Historial de Ventas:", justify=tk.LEFT, font=("Courier", 10), anchor="w", wraplength=550)
label_registro.pack()

# Ejecutar la aplicación
ventana.mainloop()
