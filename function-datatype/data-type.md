# **Tipos de Variables y Gestión de Memoria en Python - Guía Completa**

## 0. Visión General: El Modelo de Objetos de Python

Python utiliza un modelo de **todo es un objeto** con características únicas:

- **Tipado dinámico fuerte**: Los tipos se verifican en tiempo de ejecución, no en compilación
- **Gestión automática de memoria**: Garbage collector maneja la liberación de memoria
- **Sistema de referencias**: Las variables son referencias a objetos, no contenedores directos
- **Inmutabilidad vs Mutabilidad**: Impacto crucial en el comportamiento y eficiencia

---

# **1. Sistema de Tipos y Modelo de Memoria**

## 1.1 Variables como Referencias

```python
# En Python, las variables son etiquetas que apuntan a objetos
a = 10    # 'a' referencia al objeto entero 10
b = a     # 'b' referencia al MISMO objeto entero 10
a = 20    # 'a' ahora referencia a un NUEVO objeto entero 20
# 'b' sigue referenciando al objeto 10
```

### Verificando identidad de objetos
```python
a = [1, 2, 3]
b = a
c = [1, 2, 3]

print(a is b)  # True - mismo objeto
print(a is c)  # False - objetos diferentes con mismo contenido
print(a == c)  # True - contenido equivalente
```

## 1.2 Modelo de Memoria: Stack vs Heap

```python
# Stack: almacena referencias (variables)
# Heap: almacena objetos reales

def ejemplo_memoria():
    x = 10          # Referencia 'x' en stack, objeto 10 en heap
    y = [1, 2, 3]   # Referencia 'y' en stack, objeto lista en heap
    return x
```

---

# **2. Tipos Numéricos - Comportamiento y Memoria**

## 2.1 `int` - Enteros de Precisión Arbitraria

### Características de implementación:
```python
import sys

# Los enteros pequeños están pre-asignados (interning)
a = 10
b = 10
print(a is b)  # True para números pequeños (-5 a 256)

# Enteros grandes crean nuevos objetos
c = 1000
d = 1000
print(c is d)  # False - fuera del rango de interning

# Tamaño variable según magnitud
print(f"Tamaño de 10: {sys.getsizeof(10)} bytes")
print(f"Tamaño de 10**100: {sys.getsizeof(10**100)} bytes")
```

### Estructura interna aproximada:
```
Objeto int en memoria:
[Cabecera de objeto] + [valor numérico variable]
```

## 2.2 `float` - Punto Flotante de Doble Precisión

```python
# Implementación IEEE-754 (64 bits)
x = 3.14159
print(f"Tamaño float: {sys.getsizeof(x)} bytes")

# Precisión limitada - errores de representación
print(0.1 + 0.2 == 0.3)  # False
print(0.1 + 0.2)         # 0.30000000000000004

# Alternativa para precisión decimal exacta
from decimal import Decimal
preciso = Decimal('0.1') + Decimal('0.2')
print(preciso == Decimal('0.3'))  # True
```

## 2.3 `complex` - Números Complejos

```python
z = 3 + 4j
print(f"Parte real: {z.real}, Parte imaginaria: {z.imag}")
print(f"Tamaño complex: {sys.getsizeof(z)} bytes")
```

---

# **3. Cadenas de Texto (`str`) - Unicode y Eficiencia**

## 3.1 Codificación y Almacenamiento

```python
# Python 3: Unicode nativo (UTF-8 internamente)
texto = "Python 🐍"
print(f"Longitud: {len(texto)}")  # 8 caracteres
print(f"Tamaño en bytes: {sys.getsizeof(texto)}")

# Diferentes representaciones, diferentes tamaños
ascii_text = "hello"
unicode_text = "héllö"
emoji_text = "👋🌍"

print(f"ASCII: {sys.getsizeof(ascii_text)} bytes")
print(f"Unicode: {sys.getsizeof(unicode_text)} bytes") 
print(f"Emoji: {sys.getsizeof(emoji_text)} bytes")
```

## 3.2 Interning de Strings

```python
# Python optimiza strings idénticos
a = "hola"
b = "hola"
print(a is b)  # True - mismo objeto por interning

# Strings creados dinámicamente no siempre se internan
c = "".join(["h", "o", "l", "a"])
print(a is c)  # False - objeto diferente
```

---

# **4. Tipos Booleanos y Conversiones**

## 4.1 `bool` - Subtipo de Entero

```python
# True y False son instancias de bool, que hereda de int
print(isinstance(True, int))  # True
print(True == 1)              # True
print(False == 0)             # True

# Truthiness en Python
valores = [0, 1, "", "texto", [], [1], None]
for valor in valores:
    print(f"{repr(valor)} -> {bool(valor)}")
```

---

# **5. Tipos Secuenciales - Memoria y Performance**

## 5.1 `list` - Array Dinámico Mutable

### Estructura interna:
```python
import sys

lista_vacia = []
lista_10 = list(range(10))
lista_100 = list(range(100))

print(f"Lista vacía: {sys.getsizeof(lista_vacia)} bytes")
print(f"10 elementos: {sys.getsizeof(lista_10)} bytes")
print(f"100 elementos: {sys.getsizeof(lista_100)} bytes")

# Sobre-asignación de capacidad
lista = []
tamaños = []
for i in range(50):
    lista.append(i)
    tamaños.append(sys.getsizeof(lista))

# El crecimiento no es lineal - se duplica la capacidad
```

### Crecimiento de listas:
```
Tamaño lógico: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9...
Tamaño físico: 0, 4, 8, 16, 16, 25, 35, 46, 58, 72...
```

## 5.2 `tuple` - Secuencia Inmutable y Eficiente

```python
# Más eficiente en memoria que listas
lista = [1, 2, 3]
tupla = (1, 2, 3)

print(f"Lista: {sys.getsizeof(lista)} bytes")
print(f"Tupla: {sys.getsizeof(tupla)} bytes")

# Inmutabilidad permite optimizaciones
tupla1 = (1, 2, 3)
tupla2 = (1, 2, 3)
print(tupla1 is tupla2)  # Puede ser True por interning
```

## 5.3 Comparación Lista vs Tupla

```python
def comparar_rendimiento():
    import time
    
    # Tiempo de creación
    inicio = time.time()
    lista = [i for i in range(1000000)]
    tiempo_lista = time.time() - inicio
    
    inicio = time.time()
    tupla = tuple(i for i in range(1000000))
    tiempo_tupla = time.time() - inicio
    
    print(f"Creación - Lista: {tiempo_lista:.4f}s, Tupla: {tiempo_tupla:.4f}s")
    
    # Memoria
    print(f"Memoria - Lista: {sys.getsizeof(lista)} bytes, Tupla: {sys.getsizeof(tupla)} bytes")

comparar_rendimiento()
```

---

# **6. Tipos de Mapeo y Conjuntos**

## 6.1 `dict` - Tablas Hash Mutable

```python
# Estructura interna compleja - tabla hash
diccionario = {"a": 1, "b": 2, "c": 3}
print(f"Tamaño dict: {sys.getsizeof(diccionario)} bytes")

# Crecimiento dinámico
dict_pequeno = {i: i for i in range(10)}
dict_grande = {i: i for i in range(1000)}

print(f"Dict pequeño: {sys.getsizeof(dict_pequeno)} bytes")
print(f"Dict grande: {sys.getsizeof(dict_grande)} bytes")
```

## 6.2 `set` - Conjuntos Hash-Based

```python
# Implementación similar a dict pero solo con keys
conjunto = {1, 2, 3, 4, 5}
print(f"Tamaño set: {sys.getsizeof(conjunto)} bytes")

# Eficiencia en operaciones de conjunto
set1 = {1, 2, 3, 4, 5}
set2 = {4, 5, 6, 7, 8}

print(f"Unión: {set1 | set2}")
print(f"Intersección: {set1 & set2}")
print(f"Diferencia: {set1 - set2}")
```

---

# **7. Tipos Binarios - Eficiencia para Datos Crudos**

## 7.1 `bytes` - Secuencia Inmutable de Bytes

```python
# Inmutable - como tuple pero para bytes
datos = b"hello world"
print(f"Tamaño bytes: {sys.getsizeof(datos)}")
print(f"Longitud: {len(datos)}")  # 11 bytes

# Cada elemento es un entero 0-255
for byte in datos:
    print(byte, end=' ')  # 104 101 108 108 111 32 119 111 114 108 100
```

## 7.2 `bytearray` - Versión Mutable

```python
# Mutable - como list pero para bytes
buffer = bytearray(b"hello")
buffer[0] = 106  # 'j' en ASCII
print(buffer)  # bytearray(b'jello')

# Útil para protocolos de red, manipulación binaria
```

## 7.3 `memoryview` - Acceso Sin Copia

```python
# Vista de memoria sin copiar datos
datos = bytearray(b"abcdefgh")
vista = memoryview(datos)

# Slice sin copia
subvista = vista[2:5]
print(bytes(subvista))  # b'cde'

# Modificación a través de la vista
vista[0] = 122  # 'z' en ASCII
print(datos)  # bytearray(b'zbcdefgh')
```

---

# **8. Análisis Profundo de Memoria**

## 8.1 Medición Completa con `pympler` (opcional)

```python
# Para análisis más detallado
from pympler import asizeof

datos_complejos = {
    "lista": [i for i in range(100)],
    "texto": "cadena larga" * 10,
    "numero": 10**50
}

print(f"Tamaño total: {asizeof.asizeof(datos_complejos)} bytes")
```

## 8.2 Patrones de Uso de Memoria

```python
# ❌ Ineficiente - crea lista intermedia
def procesar_ineficiente(datos):
    return [x * 2 for x in datos if x > 0]

# ✅ Eficiente - usa generador
def procesar_eficiente(datos):
    return (x * 2 for x in datos if x > 0)

# ✅ Muy eficiente - evita crear estructuras
def procesar_sin_copia(datos):
    for x in datos:
        if x > 0:
            yield x * 2
```

---

# **9. Guía de Selección de Tipos**

## Decision Tree para Elegir Tipo de Colección

```
¿Necesitas una colección?
├── ¿Los datos son fijos/inmutables?
│   ├── ¿Sí? → tuple
│   └── ¿No? → list
│
├── ¿Necesitas mapear claves→valores?
│   ├── ¿Sí? → dict
│   └── ¿No? → set o list
│
├── ¿Elementos únicos sin orden?
│   ├── ¿Sí? → set
│   └── ¿No? → list
│
├── ¿Datos binarios?
│   ├── ¿Inmutable? → bytes
│   └── ¿Mutable? → bytearray
│
└── ¿Eficiencia extrema sin copias?
    └── ¿Sí? → memoryview
```

## Tabla Comparativa de Características

| Tipo | Mutabilidad | Orden | Indexable | Uso Memoria | Caso Ideal |
|------|-------------|-------|-----------|-------------|------------|
| `list` | Mutable | Sí | Sí | Alto | Colecciones modificables |
| `tuple` | Inmutable | Sí | Sí | Medio | Datos fijos, claves dict |
| `dict` | Mutable | No* | Por clave | Alto | Mapeos clave-valor |
| `set` | Mutable | No | No | Alto | Conjuntos, membresía rápida |
| `bytes` | Inmutable | Sí | Sí | Bajo | Datos binarios estáticos |
| `bytearray` | Mutable | Sí | Sí | Bajo | Buffers modificables |

*Los dict mantienen orden de inserción desde Python 3.7

---
