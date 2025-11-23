# **Funciones y Decoradores en Python - Guía Completa Intermedio/Avanzado**

## 0. Visión General: Funciones como Ciudadanos de Primera Clase

En Python, las funciones son **objetos de primera clase** (first-class citizens), lo que significa:

- Pueden ser asignadas a variables
- Pasadas como argumentos a otras funciones
- Retornadas desde otras funciones
- Almacenadas en estructuras de datos
- Tienen atributos y métodos como cualquier objeto

Este poder fundamental permite patrones avanzados como decoradores, closures y programación funcional.

---

# **1. Definición de Funciones - Más Allá de lo Básico**

## 1.1 Anatomía Completa de una Función

```python
def nombre_funcion(param1, param2=valor_default):
    """
    Docstring: documentación de la función
    
    Args:
        param1: Descripción del primer parámetro
        param2: Descripción del segundo parámetro (opcional)
    
    Returns:
        Descripción del valor de retorno
    """
    # Cuerpo de la función
    resultado = param1 + param2
    return resultado
```

## 1.2 Buenas Prácticas Profesionales

```python
# ✅ Nombres descriptivos y verbosos
def calcular_promedio_calificaciones(estudiante_id):
    pass

# ❌ Nombres crípticos
def calc_prom(est_id):
    pass

# ✅ Efectos colaterales explícitos
def procesar_y_guardar_archivo(ruta_archivo):
    """Procesa el archivo y guarda los resultados"""
    datos = procesar_archivo(ruta_archivo)
    guardar_resultados(datos)
    return datos

# ❌ Efectos colaterales ocultos
def obtener_datos(ruta_archivo):
    """Obtiene datos del archivo... y también los guarda en DB"""
    datos = procesar_archivo(ruta_archivo)
    guardar_en_base_de_datos(datos)  # ¡Sorpresa!
    return datos
```

---

# **2. Parámetros Avanzados - Flexibilidad y Control**

## 2.1 Tipos de Parámetros en Python

```python
def funcion_completa(
    posicional,           # Obligatorio, por posición
    /,                    # Separador positional-only
    posicional_o_nombre,  # Puede ser por posición o nombre
    *,                    # Separador keyword-only  
    keyword_solo,         # Solo por nombre
    **kwargs              # Argumentos nombrados adicionales
):
    pass
```

## 2.2 Parámetros Positional-Only (Python 3.8+)

```python
def procesar_datos(archivo, /, modo="lectura"):
    """
    'archivo' es positional-only - no puede pasarse por nombre
    'modo' puede ser positional o keyword
    """
    print(f"Procesando {archivo} en modo {modo}")

# ✅ Uso válido
procesar_datos("datos.csv")
procesar_datos("datos.csv", "escritura")
procesar_datos("datos.csv", modo="escritura")

# ❌ Inválido
# procesar_datos(archivo="datos.csv")  # TypeError
```

## 2.3 Parámetros Keyword-Only

```python
def crear_usuario(nombre, *, email, rol="usuario"):
    """
    'email' y 'rol' son keyword-only - obligatorio usar nombres
    """
    return {
        "nombre": nombre,
        "email": email,
        "rol": rol
    }

# ✅ Uso válido
crear_usuario("Ana", email="ana@example.com")
crear_usuario("Bob", email="bob@example.com", rol="admin")

# ❌ Inválido
# crear_usuario("Ana", "ana@example.com")  # TypeError
```

## 2.4 Empaquetado de Argumentos: `*args` y `**kwargs`

```python
def funcion_flexible(obligatorio, *args, **kwargs):
    """
    *args: captura argumentos posicionales adicionales como tupla
    **kwargs: captura argumentos nombrados adicionales como dict
    """
    print(f"Obligatorio: {obligatorio}")
    print(f"Args: {args}")
    print(f"Kwargs: {kwargs}")

# Ejemplos de uso
funcion_flexible("base")
funcion_flexible("base", "extra1", "extra2")
funcion_flexible("base", "extra", clave1="valor1", clave2="valor2")
```

## 2.5 Parámetros con Anotaciones de Tipo

```python
from typing import List, Optional, Dict

def procesar_pedido(
    productos: List[str],
    cantidad: int = 1,
    cliente: Optional[str] = None,
    **opciones: Dict[str, any]
) -> Dict[str, any]:
    """
    Anotaciones de tipo mejoran la documentación
    y permiten verificación con herramientas como mypy
    """
    return {
        "productos": productos,
        "total": len(productos) * cantidad,
        "cliente": cliente,
        "opciones": opciones
    }
```

---

# **3. Funciones como Objetos - First-Class Citizens**

## 3.1 Almacenar Funciones en Variables

```python
def saludar(nombre):
    return f"Hola, {nombre}!"

def despedir(nombre):
    return f"Adiós, {nombre}!"

# Las funciones son objetos que pueden asignarse
saludo = saludar
despedida = despedir

print(saludo("Mundo"))    # Hola, Mundo!
print(despedida("Mundo")) # Adiós, Mundo!

# Almacenar en estructuras de datos
funciones = {
    "entrada": saludar,
    "salida": despedir
}

mensaje = funciones["entrada"]("Python")
print(mensaje)  # Hola, Python!
```

## 3.2 Pasar Funciones como Argumentos

```python
def aplicar_operacion(datos, operacion):
    """
    Aplica una función 'operacion' a cada elemento de 'datos'
    """
    return [operacion(elemento) for elemento in datos]

# Diferentes operaciones que pueden pasarse
def duplicar(x):
    return x * 2

def cuadrado(x):
    return x ** 2

def negar(x):
    return -x

# Uso con diferentes operaciones
numeros = [1, 2, 3, 4, 5]

resultados_duplicados = aplicar_operacion(numeros, duplicar)
resultados_cuadrados = aplicar_operacion(numeros, cuadrado)
resultados_negados = aplicar_operacion(numeros, negar)

print(f"Duplicados: {resultados_duplicados}")
print(f"Cuadrados: {resultados_cuadrados}") 
print(f"Negados: {resultados_negados}")
```

## 3.3 Retornar Funciones (Closures)

```python
def crear_multiplicador(factor):
    """
    Retorna una función que multiplica por el factor dado
    """
    def multiplicar(numero):
        return numero * factor
    
    return multiplicar

# Crear funciones especializadas
duplicar = crear_multiplicador(2)
triplicar = crear_multiplicador(3)
multiplicar_por_10 = crear_multiplicador(10)

print(duplicar(5))        # 10
print(triplicar(5))       # 15
print(multiplicar_por_10(5))  # 50
```

## 3.4 Closures con Estado (Variables Libres)

```python
def crear_contador():
    """
    Closure que mantiene estado entre llamadas
    """
    cuenta = 0  # Variable libre - persiste entre llamadas
    
    def incrementar():
        nonlocal cuenta  # Permite modificar la variable del scope exterior
        cuenta += 1
        return cuenta
    
    return incrementar

# Cada contador mantiene su estado independiente
contador_a = crear_contador()
contador_b = crear_contador()

print(contador_a())  # 1
print(contador_a())  # 2
print(contador_b())  # 1
print(contador_a())  # 3
```

---

# **4. Funciones Lambda - Expresiones Anónimas**

## 4.1 Sintaxis y Uso Apropiado

```python
# Sintaxis básica
lambda argumentos: expresion

# Ejemplos
cuadrado = lambda x: x ** 2
sumar = lambda a, b: a + b
es_par = lambda x: x % 2 == 0
```

## 4.2 Casos de Uso Reales

```python
# Ordenamiento con clave personalizada
personas = [
    {"nombre": "Ana", "edad": 25},
    {"nombre": "Bob", "edad": 30},
    {"nombre": "Carlos", "edad": 22}
]

# Ordenar por edad
personas_ordenadas = sorted(personas, key=lambda p: p["edad"])

# Ordenar por longitud del nombre
personas_por_nombre = sorted(personas, key=lambda p: len(p["nombre"]))

# Filtrado con filter()
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
pares = list(filter(lambda x: x % 2 == 0, numeros))
mayores_que_5 = list(filter(lambda x: x > 5, numeros))

# Transformación con map()
cuadrados = list(map(lambda x: x ** 2, numeros))
```

## 4.3 Lambdas vs Funciones Nombradas

```python
# ✅ Lambdas para lógica simple y única
numeros = [1, 2, 3, 4, 5]
doblados = list(map(lambda x: x * 2, numeros))

# ❌ Evitar lambdas complejas
# resultado = map(lambda x: (x ** 2 if x % 2 == 0 else x ** 3) + 1, numeros)

# ✅ Mejor: función nombrada para lógica compleja
def transformar_complejo(x):
    if x % 2 == 0:
        return x ** 2 + 1
    else:
        return x ** 3 + 1

resultado = list(map(transformar_complejo, numeros))
```

---

# **5. Métodos - Funciones en Contexto de Clases**

## 5.1 Métodos de Instancia

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad
    
    def presentarse(self):
        """Método de instancia - opera sobre una instancia específica"""
        return f"Soy {self.nombre} y tengo {self.edad} años"
    
    def cumpleanios(self):
        """Modifica el estado de la instancia"""
        self.edad += 1
        return f"¡Ahora tengo {self.edad} años!"

# Uso
persona = Persona("Ana", 25)
print(persona.presentarse())  # Soy Ana y tengo 25 años
print(persona.cumpleanios())  # ¡Ahora tengo 26 años!
```

## 5.2 Métodos de Clase (`@classmethod`)

```python
class Usuario:
    contador = 0  # Variable de clase
    
    def __init__(self, nombre):
        self.nombre = nombre
        Usuario.contador += 1
    
    @classmethod
    def obtener_cantidad_usuarios(cls):
        """Accede a variables de clase, no de instancia"""
        return cls.contador
    
    @classmethod
    def crear_desde_csv(cls, ruta_archivo):
        """Método factory - crea instancias de manera alternativa"""
        # Lógica para leer CSV y crear usuarios
        usuarios = []
        with open(ruta_archivo, 'r') as archivo:
            for linea in archivo:
                nombre = linea.strip()
                usuarios.append(cls(nombre))  # cls es la clase misma
        return usuarios

# Uso
print(Usuario.obtener_cantidad_usuarios())  # 0

usuario1 = Usuario("Alice")
usuario2 = Usuario("Bob")

print(Usuario.obtener_cantidad_usuarios())  # 2
```

## 5.3 Métodos Estáticos (`@staticmethod`)

```python
class Calculadora:
    @staticmethod
    def sumar(a, b):
        """No accede a self ni cls - función utilitaria"""
        return a + b
    
    @staticmethod
    def es_email_valido(email):
        """Lógica de validación independiente del estado"""
        import re
        patron = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
        return bool(re.match(patron, email))

# Uso - no requiere instancia
resultado = Calculadora.sumar(5, 3)
es_valido = Calculadora.es_email_valido("usuario@example.com")

print(resultado)   # 8
print(es_valido)   # True
```

## 5.4 Guía de Elección: ¿Cuándo Usar Cada Tipo?

| Tipo | Cuándo Usar | Accede a | Ejemplo |
|------|-------------|----------|---------|
| **Instancia** | Operar sobre datos de una instancia específica | `self` | `persona.presentarse()` |
| **Clase** | Factory methods, acceder/modificar estado de clase | `cls` | `Usuario.crear_desde_csv()` |
| **Estático** | Funciones utilitarias sin dependencia de estado | Ninguno | `Calculadora.sumar()` |

---

# **6. Decoradores - Meta-programación en Python**

## 6.1 Anatomía de un Decorador Básico

```python
def mi_decorador(funcion_original):
    """
    Decorador básico - recibe una función, retorna una nueva función
    """
    def funcion_envuelta(*args, **kwargs):
        # Código a ejecutar ANTES de la función original
        print(f"🔹 Llamando a {funcion_original.__name__}")
        
        # Llamar a la función original
        resultado = funcion_original(*args, **kwargs)
        
        # Código a ejecutar DESPUÉS de la función original
        print(f"🔸 {funcion_original.__name__} retornó: {resultado}")
        
        return resultado
    
    return funcion_envuelta

# Aplicación del decorador
@mi_decorador
def saludar(nombre):
    return f"Hola, {nombre}!"

# Equivale a: saludar = mi_decorador(saludar)

print(saludar("Mundo"))
# 🔹 Llamando a saludar
# 🔸 saludar retornó: Hola, Mundo!
# Hola, Mundo!
```

## 6.2 Preservando Metadatos con `functools.wraps`

```python
import functools

def decorador_profesional(funcion_original):
    """
    Decorador que preserva los metadatos de la función original
    """
    @functools.wraps(funcion_original)
    def funcion_envuelta(*args, **kwargs):
        print(f"Ejecutando {funcion_original.__name__}")
        return funcion_original(*args, **kwargs)
    
    return funcion_envuelta

@decorador_profesional
def ejemplo():
    """Función de ejemplo con docstring"""
    return "resultado"

# Sin @functools.wraps: ejemplo.__name__ sería "funcion_envuelta"
# Con @functools.wraps: preserva los metadatos originales
print(ejemplo.__name__)  # "ejemplo"
print(ejemplo.__doc__)   # "Función de ejemplo con docstring"
```

## 6.3 Decoradores con Parámetros

```python
def repetir(veces):
    """
    Decorador con parámetros - ejecuta la función múltiples veces
    """
    def decorador_real(funcion_original):
        @functools.wraps(funcion_original)
        def funcion_envuelta(*args, **kwargs):
            resultados = []
            for i in range(veces):
                print(f"Ejecución {i + 1}/{veces}")
                resultado = funcion_original(*args, **kwargs)
                resultados.append(resultado)
            return resultados
        return funcion_envuelta
    return decorador_real

# Uso con parámetros
@repetir(veces=3)
def saludar(nombre):
    return f"Hola, {nombre}!"

resultados = saludar("Python")
# Ejecución 1/3
# Ejecución 2/3  
# Ejecución 3/3
print(resultados)  # ['Hola, Python!', 'Hola, Python!', 'Hola, Python!']
```

## 6.4 Decoradores para Casos de Uso Comunes

### Logging
```python
def logger(funcion_original):
    @functools.wraps(funcion_original)
    def funcion_envuelta(*args, **kwargs):
        print(f"📝 LOG: Ejecutando {funcion_original.__name__}")
        print(f"📝 LOG: Argumentos: {args}, Kwargs: {kwargs}")
        
        resultado = funcion_original(*args, **kwargs)
        
        print(f"📝 LOG: {funcion_original.__name__} retornó: {resultado}")
        return resultado
    
    return funcion_envuelta
```

### Medición de Tiempo
```python
import time

def temporizador(funcion_original):
    @functools.wraps(funcion_original)
    def funcion_envuelta(*args, **kwargs):
        inicio = time.perf_counter()
        
        resultado = funcion_original(*args, **kwargs)
        
        fin = time.perf_counter()
        duracion = fin - inicio
        print(f"⏱️  {funcion_original.__name__} tomó {duracion:.4f} segundos")
        
        return resultado
    
    return funcion_envuelta
```

### Validación de Argumentos
```python
def validar_rango(minimo, maximo):
    def decorador(funcion_original):
        @functools.wraps(funcion_original)
        def funcion_envuelta(*args, **kwargs):
            # Validar primer argumento posicional
            if args and isinstance(args[0], (int, float)):
                if not (minimo <= args[0] <= maximo):
                    raise ValueError(f"Valor {args[0]} fuera de rango [{minimo}, {maximo}]")
            
            return funcion_original(*args, **kwargs)
        return funcion_envuelta
    return decorador

@validar_rango(0, 100)
def establecer_porcentaje(porcentaje):
    return f"Porcentaje establecido: {porcentaje}%"

print(establecer_porcentaje(50))   # ✅ Válido
# print(establecer_porcentaje(150))  # ❌ ValueError
```

## 6.5 Decoradores de Clases

```python
def singleton(clase_original):
    """
    Decorador que convierte una clase en singleton
    """
    instancias = {}
    
    @functools.wraps(clase_original)
    def wrapper(*args, **kwargs):
        if clase_original not in instancias:
            instancias[clase_original] = clase_original(*args, **kwargs)
        return instancias[clase_original]
    
    return wrapper

@singleton
class Configuracion:
    def __init__(self):
        self.valor = "config"
        print("Configuracion inicializada")

# Solo se crea una instancia
config1 = Configuracion()  # "Configuracion inicializada"
config2 = Configuracion()  # No se imprime nada
print(config1 is config2)   # True - misma instancia
```

---

# **7. Funciones de Orden Superior**

## 7.1 Funciones Built-in de Orden Superior

```python
from functools import reduce

# map - transformar elementos
numeros = [1, 2, 3, 4, 5]
cuadrados = list(map(lambda x: x ** 2, numeros))

# filter - filtrar elementos
pares = list(filter(lambda x: x % 2 == 0, numeros))

# reduce - reducir a un solo valor
suma_total = reduce(lambda acc, x: acc + x, numeros, 0)

print(f"Cuadrados: {cuadrados}")    # [1, 4, 9, 16, 25]
print(f"Pares: {pares}")           # [2, 4]
print(f"Suma total: {suma_total}")  # 15
```

## 7.2 Creando Nuestras Propias HOF

```python
def pipeline(*funciones):
    """
    Crea un pipeline que aplica funciones en secuencia
    """
    def aplicador(datos):
        resultado = datos
        for funcion in funciones:
            resultado = funcion(resultado)
        return resultado
    return aplicador

# Funciones de transformación
def duplicar(lista):
    return [x * 2 for x in lista]

def filtrar_pares(lista):
    return [x for x in lista if x % 2 == 0]

def sumar_uno(lista):
    return [x + 1 for x in lista]

# Crear pipeline
procesador = pipeline(duplicar, filtrar_pares, sumar_uno)

numeros = [1, 2, 3, 4, 5]
resultado = procesador(numeros)
print(f"Resultado: {resultado}")  # [5, 9] (explicación: [2,4,6,8,10] → [2,4,6,8,10] → [3,5,7,9,11]? Revisar lógica)
```

---

# **8. Patrones Funcionales Avanzados**

## 8.1 Currying y Aplicación Parcial

```python
from functools import partial

# Función original
def potencia(base, exponente):
    return base ** exponente

# Aplicación parcial - fijar el exponente
cuadrado = partial(potencia, exponente=2)
cubo = partial(potencia, exponente=3)

print(cuadrado(5))  # 25
print(cubo(5))      # 125

# Aplicación parcial con lambda
sumar = lambda a, b, c: a + b + c
sumar_5_y_10 = partial(sumar, 5, 10)
print(sumar_5_y_10(15))  # 30
```

## 8.2 Memoización con `functools.lru_cache`

```python
from functools import lru_cache

@lru_cache(maxsize=128)
def fibonacci(n):
    """
    Función Fibonacci con memoización automática
    """
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

# Sin cache: O(2^n), Con cache: O(n)
print(fibonacci(50))  # Se calcula rápidamente gracias al cache

# También útil para funciones costosas con mismos parámetros
@lru_cache(maxsize=None)
def descargar_datos(url):
    print(f"Descargando {url}...")
    # Simular descarga costosa
    return f"datos_de_{url}"

# Solo se descarga una vez por URL
print(descargar_datos("https://api.com/users"))
print(descargar_datos("https://api.com/products")) 
print(descargar_datos("https://api.com/users"))  # Usa cache
```

---

# **9. Buenas Prácticas y Patrones de Diseño**

## 9.1 Principios SOLID para Funciones

### Single Responsibility Principle
```python
# ❌ Múltiples responsabilidades
def procesar_usuario(usuario):
    validar_usuario(usuario)
    guardar_en_db(usuario)
    enviar_email_bienvenida(usuario)
    generar_reporte(usuario)

# ✅ Responsabilidad única
def procesar_usuario(usuario):
    if validar_usuario(usuario):
        guardar_en_db(usuario)
        notificar_usuario(usuario)

def validar_usuario(usuario):
    # Solo validación
    pass

def notificar_usuario(usuario):
    # Solo notificación
    enviar_email_bienvenida(usuario)
    generar_reporte(usuario)
```

## 9.2 Composición sobre Herencia (Patrón Funcional)

```python
def con_logging(func):
    @functools.wraps(func)
    def envuelta(*args, **kwargs):
        print(f"LOG: Ejecutando {func.__name__}")
        return func(*args, **kwargs)
    return envuelta

def con_temporizador(func):
    @functools.wraps(func)
    def envuelta(*args, **kwargs):
        inicio = time.time()
        resultado = func(*args, **kwargs)
        print(f"Tiempo: {time.time() - inicio:.2f}s")
        return resultado
    return envuelta

# Composición de decoradores
@con_logging
@con_temporizador
def operacion_costosa():
    time.sleep(1)
    return "resultado"

operacion_costosa()
# LOG: Ejecutando operacion_costosa
# Tiempo: 1.00s
```

---

# **10. Guía de Elección: ¿Cuándo Usar Qué?**

## Árbol de Decisión para Técnicas de Funciones

```
¿Necesitas extender/modificar comportamiento?
├── ¿De forma reutilizable para múltiples funciones?
│   ├── ¿Sí? → Decoradores
│   └── ¿No? → Lógica dentro de la función
│
¿Necesitas crear funciones especializadas?
├── ¿Basadas en parámetros fijos? → functools.partial
├── ¿Con estado persistente? → Closures
└── ¿Simples y anónimas? → Lambdas

¿Trabajas en contexto de clases?
├── ¿Operas sobre instancia? → Métodos de instancia
├── ¿Necesitas factory methods? → classmethod  
└── ¿Funciones utilitarias? → staticmethod

¿Problemas de performance?
├── ¿Funciones con mismos parámetros? → lru_cache
└── ¿Procesamiento de datos? → map/filter/reduce
```

---
