```python
import bpy
import random

def crear_material(nombre, color_rgb):
    # Crea un material básico con un color específico
    mat = bpy.data.materials.new(name=nombre)
    mat.diffuse_color = (*color_rgb, 1.0) # RGBA
    return mat

def generar_escenario():
    # 1. Limpiar la escena previa
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()

    # 2. Definir materiales (Basado en modelos de color RGB)
    mat_pared_a = crear_material("ParedOscura", (0.1, 0.1, 0.1))
    mat_pared_b = crear_material("ParedDetalle", (0.8, 0.2, 0.0)) # Un naranja rojizo

    # 3. Parámetros del escenario
    largo_pasillo = 10
    ancho_pasillo = 4

    # 4. Ciclo para construir paredes (Transformación: Traslación)
    for i in range(largo_pasillo):
        # Pared Izquierda
        bpy.ops.mesh.primitive_cube_add(location=(-ancho_pasillo, i * 2, 1))
        pared_izq = bpy.context.active_object

        # Aplicar material de forma alternada (Lógica de programación)
        if i % 2 == 0:
            pared_izq.data.materials.append(mat_pared_a)
        else:
            pared_izq.data.materials.append(mat_pared_b)
            # Escalamiento para darle variedad visual
            pared_izq.scale.z = 1.5

        # Pared Derecha
        bpy.ops.mesh.primitive_cube_add(location=(ancho_pasillo, i * 2, 1))
        pared_der = bpy.context.active_object
        pared_der.data.materials.append(mat_pared_a)

    # 5. Agregar un suelo (Escalamiento y Posicionamiento)
    bpy.ops.mesh.primitive_plane_add(size=1, location=(0, (largo_pasillo - 1), 0))
    suelo = bpy.context.active_object
    suelo.scale.x = ancho_pasillo + 1
    suelo.scale.y = largo_pasillo + 1

generar_escenario()
```
