def agregar_producto():
    id_producto = input("ID del producto: ")
    nombre = input("Nombre del producto: ")
    precio = float(input("Precio del producto: "))
    cantidad = int(input("Cantidad disponible: "))

    registro = f"{id_producto}|{nombre}|{precio}|{cantidad}\n"
    with open("productos_tienda.txt", "a") as archivo:
        archivo.write(registro)
    print("Producto agregado:", registro)

def mostrar_productos():
    try:
        with open("productos_tienda.txt", "r") as archivo:
            for linea in archivo:
                print(linea.strip())
    except FileNotFoundError:
        print("No hay productos registrados.")

def buscar_producto():
    nombre_busqueda = input("Nombre del producto a buscar: ").strip().lower()
    producto_encontrado = False

    try:
        with open("productos_tienda.txt", "r") as archivo:
            for linea in archivo:
                datos = linea.strip().split("|")
                nombre = datos[1].strip().lower()
                if nombre_busqueda == nombre:
                    producto_encontrado = True
                    print("ID del Producto  : ", datos[0])
                    print("Nombre           : ", datos[1])
                    print("Precio           : $", datos[2])
                    print("Cantidad         : ", datos[3])
                    break
        if not producto_encontrado:
            print("Producto no encontrado.")
    except FileNotFoundError:
        print("No hay productos registrados.")
    input("Presiona ENTER para continuar...")

def compra_producto():
    nombre_busqueda = input("Nombre del producto a comprar: ").strip().lower()
    cantidad_comprar = int(input("Cantidad a comprar: "))
    total_compra = 0.0
    productos_actualizados = []

    try:
        with open("productos_tienda.txt", "r") as archivo:
            for linea in archivo:
                datos = linea.strip().split("|")
                nombre = datos[1].strip().lower()
                
                if nombre_busqueda == nombre:
                    precio = float(datos[2])
                    cantidad_disponible = int(datos[3])

                    if cantidad_comprar <= cantidad_disponible:
                        total_compra = precio * cantidad_comprar
                        cantidad_restante = cantidad_disponible - cantidad_comprar
                        print(f"Total a pagar: ${total_compra:.2f}")
                        datos[3] = str(cantidad_restante)
                    else:
                        print("Cantidad no disponible en inventario.")
                        return
                
                productos_actualizados.append("|".join(datos))

        # Actualizar archivo con las nuevas cantidades
        with open("productos_tienda.txt", "w") as archivo:
            for producto in productos_actualizados:
                archivo.write(producto + "\n")
        
        # Guardar el total de la compra en el archivo de ventas
        with open("total_ventas.txt", "a") as archivo_ventas:
            archivo_ventas.write(f"{total_compra:.2f}\n")
        print("Compra realizada y total registrado.")

    except FileNotFoundError:
        print("No hay productos registrados.")

def mostrar_total_ventas():
    try:
        total_ventas = 0.0
        with open("total_ventas.txt", "r") as archivo:
            for linea in archivo:
                total_ventas += float(linea.strip())
        print(f"Total de ventas acumulado: ${total_ventas:.2f}")
    except FileNotFoundError:
        print("No se ha registrado ninguna venta aún.")

# Ejecución del programa
# Puedes llamar a estas funciones según necesites:
#agregar_producto()
#mostrar_productos()
#buscar_producto()
#compra_producto()
mostrar_total_ventas()
<!---
stg1612/stg1612 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
