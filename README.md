# 24680070_Poligonos 2D
# Guía de Programación en Blender: Creación de Polígonos 2D con Python
## 1. Instalación y Configuración de Blender
Para comenzar con este ejercicio, el primer paso es instalar Blender. Sigue estas instrucciones para dejar todo listo:<br>
### Descarga e Instalación
Obtén el programa: Ve al sitio oficial de Blender y, en la sección de Download, descarga la versión recomendada para tu sistema (Windows, macOS o Linux).<br>
<img width="1771" height="886" alt="image" src="https://github.com/user-attachments/assets/00f5817a-e24d-4f5e-ba4a-349ff84ab196" />
<img width="1876" height="968" alt="image" src="https://github.com/user-attachments/assets/a7d0073e-6ea6-47ac-89d0-8c7db8af0b74" />
<br>Instalación rápida: Abre el archivo descargado y sigue los pasos del instalador haciendo clic en Next. No necesitas cambiar ninguna configuración avanzada.<br>
<img width="303" height="235" alt="image" src="https://github.com/user-attachments/assets/9aa89c83-c229-4222-9fd5-d32e1387114e" />
<br>Inicio: Una vez instalado, busca el icono de Blender en tu equipo y abre la aplicación.<br>
### Preparación del área de programación
Entra a Scripting: En la parte superior de la ventana principal, verás varias pestañas. Haz clic en la que dice Scripting. Esto cambiará la interfaz para mostrar las herramientas de programación.<br>
<img width="2427" height="2037" alt="image" src="https://github.com/user-attachments/assets/25805ed4-62f3-47fd-85d7-b0bfeebfb5bf" />
<br> Crea un archivo nuevo: Notarás un panel central que funciona como un editor de texto. Haz clic en el botón "+ New" para crear un espacio en blanco donde empezaremos a escribir nuestro código.<br>
<img width="607" height="570" alt="image" src="https://github.com/user-attachments/assets/7fecb098-839c-45ae-8045-d9689196835b" />
## 2. Configuración Inicial del Código
Luego de esto, podemos iniciar con el código importando las herramientas necesarias:<br>
```python
import bpy
import math
```
Estas líneas de código exportan las librerías necesarias para llevar a cabo el programa:<br>
bpy: Es la librería de Blender más importante. Sin ella, Python no reconocería las funciones internas ni los comandos específicos del software.<br>
math: La necesitamos para usar funciones trigonométricas (seno y coseno) y el valor de $π$, esenciales para calcular las posiciones de los vértices en un círculo.<br>
## 3. Estructura del Objeto: Malla y Contenedor
Definiremos una función llamada crear_poligono_2d donde se procesarán los atributos de nuestro objeto (nombre, lados y radio). Para que Blender entienda qué estamos creando, usamos dos conceptos clave:<br>
1) Malla (Mesh): Es la estructura básica que define la forma. Es el "lienzo" técnico donde trazaremos nuestra figura.<br>
2) Objeto: Es como un "contenedor" o caja. Tiene la información sobre la posición y rotación en el mundo 3D.<br>
```python
def crear_poligono_2d(nombre, lados, radio):
    # Crear una nueva malla y un nuevo objeto
    malla = bpy.data.meshes.new(nombre)
    objeto = bpy.data.objects.new(nombre, malla)
    
    # Vincular el objeto a la escena actual para que sea visible
    bpy.context.collection.objects.link(objeto)
    
    vertices = []
    aristas = []
```
## 4. Cálculo de Vértices y Aristas
### Determinando los puntos (Vértices)
Dentro de un ciclo for, calcularemos la posición de cada punto. Para ello, convertimos coordenadas polares a cartesianas:<br>
Ángulo: Dividimos la circunferencia completa ($2\pi$) entre el número de lados.<br>
Coordenada X: Multiplicamos el radio por el coseno (math.cos) del ángulo.<br>
Coordenada Y: Multiplicamos el radio por el seno (math.sin) del ángulo.<br>
```python
for i in range(lados):
        angulo = 2 * math.pi * i / lados
        x = radio * math.cos(angulo)
        y = radio * math.sin(angulo)
        vertices.append((x, y, 0)) # Z = 0 para mantenerlo en 2D
```
### Dibujando las líneas (Aristas)
Con los vértices ya definidos, necesitamos unirlos. Si queremos un hexágono, el ciclo se repetirá 6 veces (de 0 a 5). Para cerrar la figura, usamos la fórmula (i + 1) % lados. Esto asegura que el último punto se conecte con el primero (el punto 0), evitando errores de "punto no existe".<br>
```python
for i in range(lados):
        aristas.append((i, (i + 1) % lados))
        
    # Cargar los datos numéricos en la forma real de Blender
    malla.from_pydata(vertices, aristas, [])
    malla.update()
```
## 5. Limpieza y Ejecución
Antes de ejecutar nuestro dibujo, es recomendable limpiar la escena. Blender inicia con un cubo y una luz por defecto que podrían estorbarnos. Usamos select_all para marcar todo y delete para borrarlo.<br>
Finalmente, llamamos a nuestra función asignando los valores deseados:<br>
```python
# Limpiar la escena antes de empezar
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()

# Llamada a la función: Un hexágono de radio 5
crear_poligono_2d("Poligono2D", lados=6, radio=5)
```
## Código Completo
```python
import bpy
import math

def crear_poligono_2d(nombre, lados, radio):
    # Crear estructura de datos
    malla = bpy.data.meshes.new(nombre)
    objeto = bpy.data.objects.new(nombre, malla)
    
    # Vincular a la escena
    bpy.context.collection.objects.link(objeto)
    
    vertices = []
    aristas = []
    
    # Generar puntos del polígono
    for i in range(lados):
        angulo = 2 * math.pi * i / lados
        x = radio * math.cos(angulo)
        y = radio * math.sin(angulo)
        vertices.append((x, y, 0)) 
        
    # Crear conexiones entre puntos
    for i in range(lados):
        aristas.append((i, (i + 1) % lados))
        
    # Construir la malla final
    malla.from_pydata(vertices, aristas, [])
    malla.update()

# Preparar escena y ejecutar
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()

crear_poligono_2d("Hexagono_TAP", lados=6, radio=5)
```
Al presionar el botón "Run Script" (el icono de "Play" en el editor de texto), verás cómo aparece instantáneamente tu polígono perfectamente calculado en la vista 3D.<br>
<img width="1583" height="1013" alt="image" src="https://github.com/user-attachments/assets/b7997704-37c9-45ef-be5b-2f370199aa27" />
