# **Bucles en Python - Guía Completa Intermedio/Avanzado**

## 0. Visión General: El Ecosistema de Iteración en Python

Python ofrece un rico conjunto de herramientas para repetición que van más allá de los bucles básicos:

- **Bucles tradicionales**: `for`, `while`
- **Control de flujo**: `break`, `continue`, `else` en bucles
- **Patrones funcionales**: Comprensiones, `map`, `filter`
- **Iteración avanzada**: Generadores, iteradores personalizados
- **Utilidades profesionales**: `itertools`, patrones de procesamiento
- **Optimizaciones**: Early exit, chunk processing, lazy evaluation

---

# **1. Bucle `for` - Dominio Completo**

## 1.1 Filosofía Pythonica: Iteración sobre Elementos

A diferencia de otros lenguajes, Python prefiere **iterar directamente sobre elementos** en lugar de usar índices:

```python
# ✅ Pythonico - iterar sobre elementos
for elemento in coleccion:
    procesar(elemento)

# ❌ Menos pythonico - iterar por índices
for i in range(len(coleccion)):
    procesar(coleccion[i])
```

## 1.2 Patrones Avanzados con `for`

### a) `enumerate()` - Cuando Necesitas el Índice
```python
# Para tracking de posición y progreso
for indice, valor in enumerate(coleccion, start=1):
    print(f"Elemento {indice}: {valor}")

# Caso real: logging de procesamiento
archivos = ["data1.csv", "data2.csv", "data3.csv"]
for i, archivo in enumerate(archivos, 1):
    print(f"Procesando archivo {i}/{len(archivos)}: {archivo}")
```

### b) `zip()` - Iteración Paralela
```python
# Combinar múltiples secuencias
nombres = ["Alice", "Bob", "Charlie"]
edades = [25, 30, 35]
ciudades = ["NY", "LA", "Chicago"]

for nombre, edad, ciudad in zip(nombres, edades, ciudades):
    print(f"{nombre} ({edad}) vive en {ciudad}")

# Zip con longitud desigual - usa zip_longest de itertools
from itertools import zip_longest
lista1 = [1, 2, 3]
lista2 = ["a", "b"]
for a, b in zip_longest(lista1, lista2, fillvalue="N/A"):
    print(a, b)  # (1, 'a'), (2, 'b'), (3, 'N/A')
```

### c) Iteración sobre Diccionarios
```python
datos = {"nombre": "Ana", "edad": 28, "ciudad": "Madrid"}

# Solo claves (por defecto)
for clave in datos:
    print(clave)

# Claves y valores
for clave, valor in datos.items():
    print(f"{clave}: {valor}")

# Solo valores
for valor in datos.values():
    print(valor)
```

### d) Unpacking en el Bucle
```python
# Para estructuras de datos anidadas
coordenadas = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]
for x, y, z in coordenadas:
    print(f"Coordenada: ({x}, {y}, {z})")

# Con diccionarios que contienen estructuras conocidas
transacciones = [
    {"tipo": "compra", "monto": 100, "moneda": "USD"},
    {"tipo": "venta", "monto": 50, "moneda": "EUR"}
]

for {"tipo": t, "monto": m, "moneda": mon} in transacciones:
    print(f"{t} de {m} {mon}")
```

---

# **2. Bucle `while` - Control por Condición**

## 2.1 Casos de Uso Ideales

### a) Procesamiento hasta Condición Externa
```python
# Lectura de entrada de usuario
while (comando := input("Ingresa comando: ")) != "salir":
    print(f"Ejecutando: {comando}")
    if comando == "reiniciar":
        break

# Conexiones a servicios externos
intentos = 0
max_intentos = 3
while intentos < max_intentos:
    if conectar_servicio():
        print("Conexión exitosa")
        break
    intentos += 1
    print(f"Reintentando... ({intentos}/{max_intentos})")
else:
    print("No se pudo conectar después de múltiples intentos")
```

### b) Simulaciones y Juegos
```python
# Simulación de juego
energia = 100
while energia > 0:
    accion = input("Acción (atacar/defender/descansar): ")
    
    if accion == "atacar":
        energia -= 10
    elif accion == "defender":
        energia -= 5
    elif accion == "descansar":
        energia = min(100, energia + 20)
    
    print(f"Energía restante: {energia}")

print("Juego terminado")
```

### c) Procesamiento en Tiempo Real
```python
import time

# Bucle con timeout
tiempo_inicio = time.time()
tiempo_limite = 30  # segundos

while time.time() - tiempo_inicio < tiempo_limite:
    datos = leer_sensor()
    if datos:
        procesar(datos)
    time.sleep(1)  # Evitar uso excesivo de CPU

print("Procesamiento completado")
```

---

# **3. Control de Flujo en Bucles - Patrones Profesionales**

## 3.1 `break` - Salida Temprana Controlada

```python
# Busqueda con break temprano
def buscar_elemento(lista, objetivo):
    for i, elemento in enumerate(lista):
        if elemento == objetivo:
            print(f"Encontrado en posición {i}")
            break
        # Código adicional de procesamiento...
    else:
        print("Elemento no encontrado")
    # Código que se ejecuta siempre

# Procesamiento con condición de parada
for numero in numeros:
    if numero < 0:
        print("Número negativo detectado, deteniendo procesamiento")
        break
    procesar(numero)
```

## 3.2 `continue` - Saltar Iteraciones Específicas

```python
# Filtrar y procesar solo elementos válidos
for archivo in archivos:
    if not archivo.endswith('.csv'):
        continue  # Saltar archivos no CSV
    
    if archivo.startswith('temp_'):
        continue  # Saltar archivos temporales
    
    # Procesar solo archivos CSV válidos
    procesar_csv(archivo)

# Validación múltiple con continue
for usuario in usuarios:
    if not usuario.activo:
        continue
    if usuario.suspendido:
        continue
    if not usuario.tiene_permisos():
        continue
    
    # Solo usuarios activos, no suspendidos y con permisos
    enviar_notificacion(usuario)
```

## 3.3 `else` en Bucles - El Patrón Menos Conocido pero Útil

```python
# Búsqueda con else (sin flag variables)
def verificar_completado(tareas):
    for tarea in tareas:
        if not tarea.completada:
            print(f"Tarea pendiente: {tarea.nombre}")
            break
    else:
        # Solo se ejecuta si NO hubo break
        print("¡Todas las tareas completadas!")

# Validación de conjunto de datos
for dato in dataset:
    if not es_valido(dato):
        print("Dataset contiene datos inválidos")
        break
else:
    # Solo si todos los datos son válidos
    entrenar_modelo(dataset)

# El else se ejecuta cuando el bucle termina naturalmente
```

---

# **4. Bucles Anidados - Estrategias y Optimizaciones**

## 4.1 Patrones Comunes

```python
# Procesamiento de matrices 2D
matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

for i, fila in enumerate(matriz):
    for j, valor in enumerate(fila):
        print(f"matriz[{i}][{j}] = {valor}")

# Generación de combinaciones
colores = ["rojo", "verde", "azul"]
tamaños = ["S", "M", "L"]

print("Variantes disponibles:")
for color in colores:
    for tamaño in tamaños:
        print(f"  - {color} {tamaño}")
```

## 4.2 Optimización de Bucles Anidados

```python
# ❌ Ineficiente - operaciones repetidas
for i in range(len(lista_externa)):
    for j in range(len(lista_interna)):
        resultado = lista_externa[i] * lista_interna[j]
        print(resultado)

# ✅ Optimizado - precomputar valores
tamaño_interno = len(lista_interna)
for i, valor_externo in enumerate(lista_externa):
    for j in range(tamaño_interno):
        resultado = valor_externo * lista_interna[j]
        print(resultado)
```

---

# **5. Comprensiones - El Poder de la Expresividad**

## 5.1 List Comprehensions - Transformación y Filtrado

```python
# Transformación simple
cuadrados = [x**2 for x in range(10)]

# Filtrado
numeros_pares = [x for x in range(20) if x % 2 == 0]

# Transformación condicional
clasificacion = [
    "alto" if puntaje > 90 else "medio" if puntaje > 70 else "bajo"
    for puntaje in puntajes
]

# Múltiples iteraciones
combinaciones = [(x, y) for x in range(3) for y in range(3)]
```

## 5.2 Dict Comprehensions - Construcción de Mapeos

```python
# De lista a diccionario
nombres = ["Alice", "Bob", "Charlie"]
longitudes = {nombre: len(nombre) for nombre in nombres}

# Filtrado en comprehensions de diccionario
precios = {"manzana": 1.5, "banana": 0.8, "naranja": 1.2}
precios_actualizados = {
    fruta: precio * 1.1  # 10% de aumento
    for fruta, precio in precios.items()
    if precio > 1.0  # Solo frutas caras
}
```

## 5.3 Set Comprehensions - Colecciones Únicas

```python
# Extraer elementos únicos
texto = "programación en python"
letras_unicas = {caracter for caracter in texto if caracter.isalpha()}

# Operaciones con conjuntos
numeros = [1, 2, 2, 3, 4, 4, 5]
cuadrados_unicos = {x**2 for x in numeros}
```

## 5.4 Comprensiones Anidadas

```python
# Matriz transpuesta
matriz = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
transpuesta = [[fila[i] for fila in matriz] for i in range(3)]

# Aplanar lista de listas
lista_anidada = [[1, 2], [3, 4], [5, 6]]
plana = [elemento for sublista in lista_anidada for elemento in sublista]
```

---

# **6. Generadores e Iteradores - Eficiencia de Memoria**

## 6.1 Generadores por Función (`yield`)

```python
def generador_numeros_pares(limite):
    """Genera números pares hasta el límite"""
    for numero in range(0, limite, 2):
        yield numero

# Uso del generador
for par in generador_numeros_pares(1000000):  # ¡No ocupa memoria!
    if par > 100:
        break
    print(par)

# Generador para lectura de archivos grandes
def leer_archivo_grande(ruta):
    with open(ruta, 'r', encoding='utf-8') as archivo:
        for linea in archivo:
            yield linea.strip()

# Procesar línea por línea sin cargar todo en memoria
for linea in leer_archivo_grande("datos_gigantes.csv"):
    procesar_linea(linea)
```

## 6.2 Expresiones Generadoras

```python
# Similar a list comprehension pero perezoso
cuadrados = (x**2 for x in range(1000000))

# Eficiente para pipelines de procesamiento
suma_cuadrados = sum(x**2 for x in range(1000) if x % 2 == 0)
```

## 6.3 Iteradores Personalizados

```python
class ContadorRegresivo:
    def __init__(self, inicio):
        self.actual = inicio
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.actual < 0:
            raise StopIteration
        valor_actual = self.actual
        self.actual -= 1
        return valor_actual

# Uso
for numero in ContadorRegresivo(5):
    print(numero)  # 5, 4, 3, 2, 1, 0
```

---

# **7. `itertools` - Herramientas Profesionales de Iteración**

## 7.1 Combinatorias

```python
from itertools import permutations, combinations, product

# Permutaciones y combinaciones
letras = ['A', 'B', 'C']
print("Permutaciones:")
for p in permutations(letras, 2):
    print(p)  # ('A','B'), ('A','C'), ('B','A'), etc.

print("Combinaciones:")
for c in combinations(letras, 2):
    print(c)  # ('A','B'), ('A','C'), ('B','C')

# Producto cartesiano
colores = ['rojo', 'azul']
tamaños = ['S', 'L']
for color, tamaño in product(colores, tamaños):
    print(f"{color} {tamaño}")
```

## 7.2 Iteración Infinita Controlada

```python
from itertools import count, cycle, islice

# Contador infinito
for i in count(start=10, step=2):
    if i > 20:
        break
    print(i)  # 10, 12, 14, 16, 18, 20

# Ciclo infinito con límite
elementos = ['A', 'B', 'C']
for i, elem in enumerate(cycle(elementos)):
    if i >= 10:
        break
    print(elem)

# Slice de iteradores infinitos
primeros_5 = list(islice(count(), 5))  # [0, 1, 2, 3, 4]
```

## 7.3 Agrupación y Filtrado Avanzado

```python
from itertools import groupby, takewhile, dropwhile

# Agrupar por clave
datos = [
    {'nombre': 'Ana', 'edad': 25},
    {'nombre': 'Bob', 'edad': 25},
    {'nombre': 'Carlos', 'edad': 30}
]

for edad, grupo in groupby(datos, key=lambda x: x['edad']):
    print(f"Edad {edad}: {list(grupo)}")

# takewhile y dropwhile
numeros = [1, 3, 5, 2, 4, 6]
menores_que_4 = list(takewhile(lambda x: x < 4, numeros))  # [1, 3]
resto = list(dropwhile(lambda x: x < 4, numeros))  # [5, 2, 4, 6]
```

---

# **8. Patrones Profesionales y Optimizaciones**

## 8.1 Early Exit y Validación

```python
# Validación temprana en procesamiento por lotes
def procesar_lote(datos):
    for elemento in datos:
        if not es_valido(elemento):
            print(f"Elemento inválido encontrado: {elemento}")
            return False  # Salir temprano de toda la función
    
    # Procesar solo si todos son válidos
    for elemento in datos:
        procesar_elemento(elemento)
    
    return True
```

## 8.2 Procesamiento por Chunks

```python
def procesar_por_lotes(datos, tamaño_lote=1000):
    """Procesa datos en lotes para evitar sobrecarga de memoria"""
    for i in range(0, len(datos), tamaño_lote):
        lote = datos[i:i + tamaño_lote]
        print(f"Procesando lote {i//tamaño_lote + 1}")
        yield procesar_lote(lote)

# Uso con generador
for resultado in procesar_por_lotes(grandes_datos, tamaño_lote=500):
    guardar_resultado(resultado)
```

## 8.3 Bucles con Estado Acumulativo

```python
# Acumulación con estado complejo
def analizar_secuencia(numeros):
    max_actual = float('-inf')
    min_actual = float('inf')
    suma_total = 0
    
    for i, numero in enumerate(numeros, 1):
        max_actual = max(max_actual, numero)
        min_actual = min(min_actual, numero)
        suma_total += numero
        
        if i % 1000 == 0:
            print(f"Procesados {i} elementos")
            print(f"Rango actual: {min_actual}-{max_actual}")
    
    return {
        "max": max_actual,
        "min": min_actual,
        "promedio": suma_total / len(numeros)
    }
```

## 8.4 Patrón: Pipeline de Procesamiento

```python
def pipeline_procesamiento(datos):
    # Filtrado
    filtrados = (d for d in datos if d['activo'])
    
    # Transformación
    transformados = (transformar(d) for d in filtrados)
    
    # Agrupación (usando itertools)
    from itertools import groupby
    agrupados = groupby(transformados, key=lambda x: x['categoria'])
    
    # Resultado final
    for categoria, grupo in agrupados:
        yield categoria, list(grupo)
```

---

# **9. Guía de Elección: ¿Cuándo Usar Qué?**

## Árbol de Decisión para Estructuras de Repetición

```
¿Necesitas repetir operaciones?
├── ¿Conoces el número de iteraciones?
│   ├── ¿Sí? → `for`
│   └── ¿No? → `while`
│
├── ¿Necesitas crear una nueva colección?
│   ├── ¿Sí? → Comprensiones
│   └── ¿No? → Bucles tradicionales
│
├── ¿Trabajas con datasets grandes?
│   ├── ¿Sí? → Generadores
│   └── ¿No? → Listas normales
│
├── ¿Necesitas combinaciones/permutaciones?
│   ├── ¿Sí? → `itertools`
│   └── ¿No? → Bucles anidados
│
└── ¿Buscas optimizar memoria/performance?
    ├── ¿Sí? → Generadores + `itertools`
    └── ¿No? → Enfoque más simple
```

## Tabla Comparativa de Herramientas

| Escenario | Herramienta Recomendada | Razón |
|-----------|------------------------|-------|
| Iteración sobre elementos conocidos | `for` + `enumerate`/`zip` | Legibilidad y control |
| Condiciones de parada dinámicas | `while` + condición | Flexibilidad |
| Transformación de colecciones | Comprensiones | Concisión y velocidad |
| Procesamiento de datos grandes | Generadores | Eficiencia de memoria |
| Combinatorias matemáticas | `itertools` | Correctitud y performance |
| Validaciones y búsquedas | `for` + `break`/`else` | Control de flujo claro |
| Pipelines de datos | Generadores + `itertools` | Composición y lazy evaluation |

---

