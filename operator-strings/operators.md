# **Operadores en Python - Guía Completa Intermedio/Avanzado**

## 0. Visión General: El Ecosistema de Operadores en Python

Python ofrece un conjunto rico de operadores que van más allá de la aritmética básica:

- **Operadores tradicionales**: Aritméticos, comparación, lógicos
- **Operadores de identidad y pertenencia**: `is`, `in`
- **Operadores a nivel de bits**: Para manipulación binaria
- **Operadores de asignación compuesta**: Combinación de operación y asignación
- **Operadores especiales**: Walrus, unpacking, sobrecarga
- **Patrones avanzados**: Encadenamiento, short-circuit evaluation, operadores mágicos

---

# **1. Operadores Aritméticos - Más Allá de lo Básico**

## 1.1 Set Completo y Comportamientos

```python
# Operadores básicos
a + b    # Suma
a - b    # Resta  
a * b    # Multiplicación
a / b    # División flotante
a // b   # División entera (floor division)
a % b    # Módulo (resto)
a ** b   # Potencia
-a       # Negación
+a       # Positivo (generalmente no cambia nada)
abs(a)   # Valor absoluto
```

## 1.2 Comportamientos Específicos por Tipo

```python
# Con enteros
print(7 / 2)    # 3.5 - siempre float
print(7 // 2)   # 3 - división entera
print(-7 // 2)  # -4 - cuidado: floor hacia -∞
print(7 % 2)    # 1 - resto
print(-7 % 2)   # 1 - importante para cálculos cíclicos

# Con strings
print("a" + "b")        # "ab" - concatenación
print("a" * 3)          # "aaa" - repetición

# Con listas
print([1, 2] + [3, 4])  # [1, 2, 3, 4] - concatenación
print([0] * 5)          # [0, 0, 0, 0, 0] - repetición
```

## 1.3 División y Módulo - Comportamiento con Negativos

```python
def explicar_division(a, b):
    print(f"{a} // {b} = {a // b}")
    print(f"{a} % {b} = {a % b}")
    print(f"Verificación: ({b} * {a // b}) + {a % b} = {b * (a // b) + a % b}")

explicar_division(7, 3)    # 7 // 3 = 2, 7 % 3 = 1
explicar_division(-7, 3)   # -7 // 3 = -3, -7 % 3 = 2
explicar_division(7, -3)   # 7 // -3 = -3, 7 % -3 = -2
explicar_division(-7, -3)  # -7 // -3 = 2, -7 % -3 = -1
```

## 1.4 Aplicaciones en Comprensiones y Mapas

```python
# Transformación de listas sin bucles explícitos
numeros = [1, 2, 3, 4, 5]

# Potencias al cubo
cubos = [x ** 3 for x in numeros]
print(f"Cubos: {cubos}")  # [1, 8, 27, 64, 125]

# Operaciones vectoriales implícitas
precios = [100, 200, 300]
descuentos = [precio * 0.9 for precio in precios]  # 10% de descuento
incrementos = [precio * 1.21 for precio in precios]  # 21% de IVA

print(f"Precios con descuento: {descuentos}")
print(f"Precios con IVA: {incrementos}")
```

---

# **2. Operadores de Comparación - Encadenamiento y Más**

## 2.1 Set Completo de Comparadores

```python
a == b   # Igualdad
a != b   # Desigualdad
a < b    # Menor que
a > b    # Mayor que  
a <= b   # Menor o igual que
a >= b   # Mayor o igual que
```

## 2.2 Encadenamiento de Comparaciones (Pythonic)

```python
# ❌ Menos legible
if x > 10 and x < 20 and y != 5:

# ✅ Pythonic - comparaciones encadenadas
if 10 < x < 20 != y:
    print("x entre 10 y 20, y diferente de 20")

# Múltiples variables y operadores
a, b, c = 5, 10, 15
if a < b < c:
    print("a < b < c")  # Se evalúa como (a < b) and (b < c)

# Con diferentes operadores
edad = 25
if 18 <= edad < 65:
    print("Edad laboral")

# Validación de rangos complejos
temperatura = 22
humedad = 60
if 20 <= temperatura <= 25 and 40 <= humedad <= 70:
    print("Condiciones óptimas")
```

## 2.3 Comparación entre Diferentes Tipos

```python
# Python 3 no permite comparación entre tipos incompatibles
print(3 < 4.5)     # True - int y float
print("3" < "4")   # True - comparación lexicográfica

# ❌ Esto genera TypeError en Python 3
# print(3 < "4")  # TypeError: '<' not supported between 'int' and 'str'

# Orden de tipos (para ordenamiento heterogéneo)
# None < números < strings < tuplas < listas < ...
```

---

# **3. Operadores Lógicos y Short-Circuit Evaluation**

## 3.1 Operadores Básicos

```python
a and b   # AND lógico
a or b    # OR lógico  
not a     # NOT lógico
```

## 3.2 Short-Circuit Evaluation (Evaluación de Cortocircuito)

```python
def funcion_costosa():
    print("Ejecutando función costosa...")
    return True

# ❌ Siempre ejecuta ambas funciones
if funcion_costosa() & funcion_costosa():  # Bitwise AND, no short-circuit
    pass

# ✅ Short-circuit - si la primera es False, no evalúa la segunda
if False and funcion_costosa():
    pass  # No se imprime nada

# ✅ Short-circuit - si la primera es True, no evalúa la segunda  
if True or funcion_costosa():
    pass  # No se imprime nada
```

## 3.3 Patrones Profesionales con Short-Circuit

```python
# Validación segura de atributos anidados
usuario = None
# if usuario and usuario.direccion and usuario.direccion.ciudad:
#     print(usuario.direccion.ciudad)

# Asignación con valor por defecto
config = None
valor = config or "valor_por_defecto"
print(valor)  # "valor_por_defecto"

# Procesamiento condicional
lista = None
elementos = lista or []
for elemento in elementos:
    procesar(elemento)
```

## 3.4 Operador Ternario vs Lógicos

```python
# Operador ternario (claro pero más verboso)
estado = "activo" if usuario.conectado else "inactivo"

# Usando lógicos (compacto pero menos explícito)
estado = usuario.conectado and "activo" or "inactivo"

# Cuidado con valores falsy
nombre = "" and "anónimo" or "desconocido"  # ¡Devuelve "desconocido"!
# Mejor con ternario:
nombre = "anónimo" if not nombre else nombre
```

---

# **4. Operadores de Asignación Compuesta**

## 4.1 Set Completo

```python
a += b    # a = a + b
a -= b    # a = a - b
a *= b    # a = a * b
a /= b    # a = a / b
a //= b   # a = a // b
a %= b    # a = a % b
a **= b   # a = a ** b
a &= b    # a = a & b
a |= b    # a = a | b
a ^= b    # a = a ^ b
a <<= b   # a = a << b
a >>= b   # a = a >> b
```

## 4.2 Casos de Uso Comunes

```python
# Acumuladores
total = 0
for numero in [1, 2, 3, 4, 5]:
    total += numero
print(f"Total: {total}")  # 15

# Concatenación de strings
mensaje = "Hola"
mensaje += " "
mensaje += "Mundo"
print(mensaje)  # "Hola Mundo"

# Construcción de listas
resultados = []
for i in range(5):
    resultados += [i * 2]  # Equivale a resultados.extend([i * 2])
print(resultados)  # [0, 2, 4, 6, 8]

# Contadores de frecuencia
frecuencias = {}
texto = "python programming"
for letra in texto:
    if letra != " ":  # Ignorar espacios
        frecuencias[letra] = frecuencias.get(letra, 0) + 1

print(f"Frecuencias: {frecuencias}")
```

---

# **5. Operadores Bitwise - Manipulación a Nivel de Bits**

## 5.1 Operadores Básicos

```python
a & b   # AND - bits que son 1 en ambos
a | b   # OR - bits que son 1 en alguno
a ^ b   # XOR - bits que son 1 en uno pero no ambos  
~a      # NOT - invertir todos los bits
a << n  # Desplazamiento izquierda (multiplica por 2^n)
a >> n  # Desplazamiento derecha (divide por 2^n)
```

## 5.2 Ejemplos Prácticos

```python
# Representación binaria
def mostrar_bits(numero, bits=8):
    return f"{numero:0{bits}b} ({numero})"

a = 0b1010  # 10
b = 0b1100  # 12

print(f"a = {mostrar_bits(a)}")
print(f"b = {mostrar_bits(b)}")
print(f"a & b = {mostrar_bits(a & b)}")  # 0b1000 (8)
print(f"a | b = {mostrar_bits(a | b)}")  # 0b1110 (14)
print(f"a ^ b = {mostrar_bits(a ^ b)}")  # 0b0110 (6)
print(f"~a = {mostrar_bits(~a & 0xFF, 8)}")  # 0b11110101 (245) - mask para 8 bits
print(f"a << 2 = {mostrar_bits(a << 2)}")  # 0b101000 (40)
print(f"b >> 1 = {mostrar_bits(b >> 1)}")  # 0b0110 (6)
```

## 5.3 Sistema de Permisos con Flags Bitwise

```python
# Definición de permisos
LECTURA = 0b001   # 1
ESCRITURA = 0b010  # 2  
EJECUCION = 0b100  # 4

class SistemaPermisos:
    def __init__(self):
        self.permisos = {}
    
    def asignar_permisos(self, usuario, *flags):
        """Asigna permisos usando OR bitwise"""
        permiso = 0
        for flag in flags:
            permiso |= flag
        self.permisos[usuario] = permiso
    
    def tiene_permiso(self, usuario, flag):
        """Verifica permiso usando AND bitwise"""
        return bool(self.permisos.get(usuario, 0) & flag)
    
    def mostrar_permisos(self, usuario):
        permiso = self.permisos.get(usuario, 0)
        return {
            'lectura': bool(permiso & LECTURA),
            'escritura': bool(permiso & ESCRITURA),
            'ejecucion': bool(permiso & EJECUCION)
        }

# Uso del sistema
sistema = SistemaPermisos()
sistema.asignar_permisos("admin", LECTURA, ESCRITURA, EJECUCION)
sistema.asignar_permisos("usuario", LECTURA, ESCRITURA)

print(sistema.mostrar_permisos("admin"))   # Todos True
print(sistema.mostrar_permisos("usuario")) # lectura y escritura True
print(sistema.tiene_permiso("usuario", EJECUCION))  # False
```

## 5.4 Operaciones Bitwise Avanzadas

```python
# Verificar si un número es par/impar
def es_par(n):
    return (n & 1) == 0

# Intercambiar valores sin variable temporal
a, b = 5, 10
a ^= b
b ^= a
a ^= b
print(f"a={a}, b={b}")  # a=10, b=5

# Extraer bit específico
def obtener_bit(numero, posicion):
    return (numero >> posicion) & 1

# Establecer bit específico
def establecer_bit(numero, posicion):
    return numero | (1 << posicion)

# Limpiar bit específico  
def limpiar_bit(numero, posicion):
    return numero & ~(1 << posicion)

# Invertir bit específico
def invertir_bit(numero, posicion):
    return numero ^ (1 << posicion)
```

---

# **6. Operadores de Identidad - `is` vs `==`**

## 6.1 Diferencias Fundamentales

```python
# == compara VALORES
# is compara IDENTIDAD DE OBJETOS (misma ubicación en memoria)

a = [1, 2, 3]
b = [1, 2, 3]  # Mismo contenido, objeto diferente
c = a          # Mismo objeto

print(a == b)  # True - mismos valores
print(a is b)  # False - objetos diferentes
print(a is c)  # True - mismo objeto

# Verificación de identidad
print(id(a))  # Dirección de memoria de a
print(id(b))  # Dirección diferente
print(id(c))  # Misma dirección que a
```

## 6.2 Casos de Uso Apropiados para `is`

```python
# Comparar con singleton None
valor = None
if valor is None:  # ✅ Correcto
    print("Valor es None")

if valor == None:  # ❌ Funciona pero no es pythonic
    print("También funciona pero no es ideal")

# Comparar con booleanos
if resultado is True:   # ✅ Cuando quieres exactamente True, no truthy
    print("Exactamente True")

if resultado == True:   # ❌ Menos claro
    print("También funciona")

# Verificar si es el mismo objeto
original = objeto
copia = hacer_copia(objeto)
if original is copia:
    print("Es la misma instancia")
```

## 6.3 Interning de Strings y Enteros

```python
# Python optimiza strings pequeños e enteros comunes
a = "hola"
b = "hola"
print(a is b)  # True - por interning

c = "hola mundo!"
d = "hola mundo!"  
print(c is d)  # False - string demasiado largo

# Enteros pequeños (-5 a 256) están internados
x = 100
y = 100
print(x is y)  # True

z = 1000
w = 1000  
print(z is w)  # False - fuera del rango de interning
```

---

# **7. Operadores de Pertenencia - `in` y `not in`**

## 7.1 Uso con Diferentes Tipos

```python
# Strings
texto = "python programming"
print("python" in texto)    # True
print("java" not in texto)  # True

# Listas
frutas = ["manzana", "banana", "naranja"]
print("manzana" in frutas)    # True
print("uva" not in frutas)    # True

# Tuplas
coordenadas = (10, 20, 30)
print(20 in coordenadas)  # True

# Diccionarios (busca en claves)
persona = {"nombre": "Ana", "edad": 25}
print("nombre" in persona)    # True
print("Ana" in persona)       # False - busca en claves, no valores
```

## 7.2 Validaciones Limpias con `in`

```python
# Validación de roles
def tiene_acceso(usuario):
    roles_permitidos = {"admin", "editor", "moderador"}
    return usuario.rol in roles_permitidos

# Validación de extensiones de archivo
def es_imagen(nombre_archivo):
    extensiones_validas = {".jpg", ".png", ".gif", ".bmp"}
    return any(nombre_archivo.lower().endswith(ext) for ext in extensiones_validas)

# Comandos válidos en un sistema
comando = input("Ingrese comando: ")
comandos_validos = {"start", "stop", "restart", "status"}

if comando in comandos_validos:
    ejecutar_comando(comando)
else:
    print(f"Comando inválido. Use: {', '.join(comandos_validos)}")
```

---

# **8. Operadores Avanzados y Patrones Profesionales**

## 8.1 Unpacking con `*` y `**`

```python
# Unpacking de secuencias
a, b, c = [1, 2, 3]
primero, *resto = [1, 2, 3, 4, 5]
print(primero)  # 1
print(resto)    # [2, 3, 4, 5]

# Unpacking en llamadas a funciones
def funcion(a, b, c):
    return a + b + c

datos = [1, 2, 3]
print(funcion(*datos))  # 6

# Unpacking de diccionarios
config = {"host": "localhost", "port": 8080, "debug": True}
def conectar(host, port, debug=False):
    print(f"Conectando a {host}:{port} (debug: {debug})")

conectar(**config)

# Unpacking en asignaciones
punto = (10, 20)
x, y = punto
```

## 8.2 Operador Walrus `:=` (Python 3.8+)

```python
# Asignación en expresiones
while (linea := input("Ingrese texto: ")) != "salir":
    print(f"Escribiste: {linea}")

# En comprensiones con filtro
datos = [1, 2, 3, 4, 5]
cuadrados = [cuadrado for x in datos if (cuadrado := x ** 2) > 10]
print(cuadrados)  # [16, 25]

# Evitar cálculo duplicado
if (longitud := len("texto largo")) > 5:
    print(f"Texto muy largo: {longitud} caracteres")
```

## 8.3 Sobrecarga de Operadores con Métodos Mágicos

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):
        """Sobrecarga del operador +"""
        if isinstance(other, Vector):
            return Vector(self.x + other.x, self.y + other.y)
        elif isinstance(other, (int, float)):
            return Vector(self.x + other, self.y + other)
        return NotImplemented
    
    def __mul__(self, other):
        """Sobrecarga del operador * (multiplicación por escalar)"""
        if isinstance(other, (int, float)):
            return Vector(self.x * other, self.y * other)
        return NotImplemented
    
    def __eq__(self, other):
        """Sobrecarga del operador =="""
        if isinstance(other, Vector):
            return self.x == other.x and self.y == other.y
        return False
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"

# Uso
v1 = Vector(2, 3)
v2 = Vector(1, 1)

print(v1 + v2)        # Vector(3, 4)
print(v1 * 3)         # Vector(6, 9)
print(v1 == Vector(2, 3))  # True
```

## 8.4 Operadores en Comprensiones Avanzadas

```python
# Con filtro condicional
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
pares_al_cuadrado = [x ** 2 for x in numeros if x % 2 == 0]

# Con operador ternario
clasificaciones = [
    "positivo" if x > 0 else "cero" if x == 0 else "negativo"
    for x in [-2, -1, 0, 1, 2]
]

# Comprensión de diccionarios con ternario
numeros = [1, 2, 3, 4, 5]
resultados = {
    x: "par" if x % 2 == 0 else "impar"
    for x in numeros
}

# Comprensión de sets
palabras = ["python", "java", "python", "c++"]
longitudes_unicas = {len(palabra) for palabra in palabras}
```

---

# **9. Operadores con Funciones Built-in**

## 9.1 `any()` y `all()` - Operadores Lógicos Aplicados

```python
# any() - equivalente a una serie de OR
numeros = [0, 0, 0, 1, 0]
print(any(numeros))  # True - hay al menos un True (1)

# all() - equivalente a una serie de AND  
booleanos = [True, True, False, True]
print(all(booleanos))  # False - no todos son True

# Casos prácticos
usuarios = [{"activo": True}, {"activo": False}, {"activo": True}]
todos_activos = all(user["activo"] for user in usuarios)
alguno_activo = any(user["activo"] for user in usuarios)

print(f"Todos activos: {todos_activos}")    # False
print(f"Algún activo: {alguno_activo}")     # True
```

## 9.2 Operadores con `map()`, `filter()`, `reduce()`

```python
from functools import reduce

# map con operadores
numeros = [1, 2, 3, 4, 5]
dobles = list(map(lambda x: x * 2, numeros))

# filter con operadores
pares = list(filter(lambda x: x % 2 == 0, numeros))

# reduce con operadores
producto = reduce(lambda x, y: x * y, numeros)
suma = reduce(lambda x, y: x + y, numeros)

print(f"Dobles: {dobles}")      # [2, 4, 6, 8, 10]
print(f"Pares: {pares}")        # [2, 4]
print(f"Producto: {producto}")  # 120
print(f"Suma: {suma}")          # 15
```

---

# **10. Guía de Elección: ¿Cuándo Usar Qué Operador?**

## Árbol de Decisión para Operadores

```
¿Necesitas comparar valores?
├── ¿Comparar identidad de objetos? → `is`, `is not`
├── ¿Verificar pertenencia? → `in`, `not in`
└── ¿Comparar valores? → `==`, `!=`, `<`, `>`, etc.

¿Necesitas operaciones matemáticas?
├── ¿Enteras? → `//`, `%`
├── ¿Punto flotante? → `/`
├── ¿Potencia? → `**`
└── ¿Bits? → `&`, `|`, `^`, `<<`, `>>`

¿Necesitas lógica booleana?
├── ¿Con short-circuit? → `and`, `or`
├── ¿Sin short-circuit? → `&`, `|` (bitwise)
└── ¿Negación? → `not`

¿Necesitas asignar y operar?
└── ¿Sí? → `+=`, `-=`, `*=`, etc.
```

## Tabla de Operadores por Categoría

| Categoría | Operadores | Casos de Uso |
|-----------|------------|--------------|
| **Aritméticos** | `+ - * / // % **` | Cálculos matemáticos |
| **Comparación** | `== != < > <= >=` | Ordenamiento, validaciones |
| **Lógicos** | `and or not` | Condiciones complejas |
| **Bitwise** | `& | ^ ~ << >>` | Flags, manipulación binaria |
| **Asignación** | `= += -= *= etc.` | Acumuladores, contadores |
| **Identidad** | `is is not` | Singletons (None, True, False) |
| **Pertenencia** | `in not in` | Búsqueda en colecciones |
| **Unpacking** | `* **` | Manipulación de secuencias |

---

