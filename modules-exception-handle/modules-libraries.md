# **Modularización y Bibliotecas Estándar de Python - Guía Completa Intermedio/Avanzado**

## 0. Visión General: El Ecosistema de Python

Python se destaca por su filosofía "batteries included" (pilas incluidas), ofreciendo una biblioteca estándar extensa que cubre prácticamente todas las necesidades comunes de programación:

- **Modularización**: Organización eficiente de código en módulos y paquetes
- **Manipulación de tiempo**: `datetime`, `time`, `calendar` para operaciones temporales
- **Matemáticas y estadísticas**: `math`, `statistics`, `random` para cálculos y análisis
- **Sistema de archivos**: `os`, `pathlib`, `shutil` para operaciones del sistema
- **Estructuras de datos**: `collections`, `itertools` para contenedores avanzados
- **Serialización**: `json`, `pickle` para persistencia de datos
- **Utilidades funcionales**: `functools`, `operator` para programación funcional

---

# **1. Modularización en Python - Arquitectura Profesional**

## 1.1 Fundamentos de Importación

```python
# Importación absoluta (recomendada)
import modulo
import paquete.modulo
from paquete import modulo
from paquete.modulo import funcion

# Importación relativa (dentro de paquetes)
from . import modulo_hermano
from .. import modulo_padre
from .subpaquete import modulo_hijo

# Aliasing para claridad o evitar conflictos
import pandas as pd
import numpy as np
from datetime import datetime as dt
```

## 1.2 Estructura de Proyecto Profesional

```
mi_proyecto/
├── README.md
├── requirements.txt
├── setup.py
├── src/
│   └── mi_paquete/
│       ├── __init__.py
│       ├── core/
│       │   ├── __init__.py
│       │   ├── calculos.py
│       │   └── validaciones.py
│       ├── utils/
│       │   ├── __init__.py
│       │   ├── fechas.py
│       │   └── textos.py
│       └── cli/
│           ├── __init__.py
│           └── comandos.py
├── tests/
│   ├── __init__.py
│   ├── test_calculos.py
│   └── test_utils.py
└── scripts/
    └── ejecutar.py
```

## 1.3 Creación de Módulos Especializados

**`src/mi_paquete/utils/textos.py`**
```python
"""Módulo de utilidades para manipulación de textos"""

def normalizar_texto(texto: str) -> str:
    """Normaliza texto: minúsculas, sin espacios extra"""
    return ' '.join(texto.lower().split())

def contar_palabras(texto: str) -> dict:
    """Cuenta frecuencia de palabras en un texto"""
    palabras = normalizar_texto(texto).split()
    return {palabra: palabras.count(palabra) for palabra in set(palabras)}

def es_palindromo(texto: str) -> bool:
    """Verifica si un texto es palíndromo"""
    texto_limpio = ''.join(c for c in texto.lower() if c.isalnum())
    return texto_limpio == texto_limpio[::-1]
```

**`src/mi_paquete/utils/numeros.py`**
```python
"""Módulo de utilidades para operaciones numéricas"""

def es_primo(n: int) -> bool:
    """Verifica si un número es primo"""
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True

def fibonacci(n: int) -> list:
    """Genera secuencia de Fibonacci hasta n términos"""
    if n <= 0:
        return []
    elif n == 1:
        return [0]
    secuencia = [0, 1]
    for _ in range(2, n):
        secuencia.append(secuencia[-1] + secuencia[-2])
    return secuencia

def factorial(n: int) -> int:
    """Calcula factorial de un número"""
    if n < 0:
        raise ValueError("Factorial no definido para números negativos")
    resultado = 1
    for i in range(1, n + 1):
        resultado *= i
    return resultado
```

## 1.4 Configuración de Paquete con `__init__.py`

**`src/mi_paquete/utils/__init__.py`**
```python
"""
Paquete de utilidades para el proyecto

Exporta funciones principales para uso externo
"""

from .textos import normalizar_texto, contar_palabras, es_palindromo
from .numeros import es_primo, fibonacci, factorial

# Definir qué se exporta con import *
__all__ = [
    'normalizar_texto',
    'contar_palabras', 
    'es_palindromo',
    'es_primo',
    'fibonacci',
    'factorial'
]

# Metadata del paquete
__version__ = '1.0.0'
__author__ = 'Tu Nombre'
```

## 1.5 Uso del Paquete desde Código Externo

```python
# Desde el directorio raíz del proyecto
import sys
sys.path.append('src')  # Para desarrollo, en producción usar pip install

from mi_paquete.utils import (
    normalizar_texto, 
    es_primo, 
    fibonacci
)

# Uso de las funciones
texto = "   Hola   Mundo  Python   "
print(normalizar_texto(texto))  # "hola mundo python"

print(es_primo(17))  # True
print(fibonacci(10))  # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

---

# **2. Manejo de Fechas y Tiempos - `datetime` Profundo**

## 2.1 Tipos Fundamentales de `datetime`

```python
from datetime import datetime, date, time, timedelta

# datetime - fecha y hora completa
ahora = datetime.now()
fecha_especifica = datetime(2024, 12, 25, 15, 30, 45)

# date - solo fecha
hoy = date.today()
navidad = date(2024, 12, 25)

# time - solo hora
hora_actual = ahora.time()
hora_especifica = time(14, 30, 15)

# timedelta - diferencias temporales
un_dia = timedelta(days=1)
dos_horas = timedelta(hours=2)
```

## 2.2 Formateo y Parseo Avanzado

```python
from datetime import datetime

# Formateo (datetime → string)
ahora = datetime.now()
print(ahora.strftime("%Y-%m-%d %H:%M:%S"))        # "2024-01-15 14:30:25"
print(ahora.strftime("%A %d de %B de %Y"))        # "Monday 15 de January de 2024"
print(ahora.strftime("ISO: %Y-%m-%dT%H:%M:%SZ"))  # "ISO: 2024-01-15T14:30:25Z"

# Parseo (string → datetime)
fecha1 = datetime.strptime("2024-12-25", "%Y-%m-%d")
fecha2 = datetime.strptime("25/12/2024 15:30", "%d/%m/%Y %H:%M")
fecha3 = datetime.strptime("Dec 25, 2024", "%b %d, %Y")

# Formateo localizado
def formatear_fecha_espanol(fecha):
    meses = [
        'enero', 'febrero', 'marzo', 'abril', 'mayo', 'junio',
        'julio', 'agosto', 'septiembre', 'octubre', 'noviembre', 'diciembre'
    ]
    dias_semana = [
        'lunes', 'martes', 'miércoles', 'jueves', 
        'viernes', 'sábado', 'domingo'
    ]
    return fecha.strftime(f"%A %d de {meses[fecha.month-1]} de %Y")

print(formatear_fecha_espanol(ahora))
```

## 2.3 Operaciones Temporales Complejas

```python
from datetime import datetime, timedelta

def calcular_dias_entre(fecha1, fecha2):
    """Calcula días entre dos fechas"""
    return abs((fecha2 - fecha1).days)

def agregar_dias_habiles(fecha, dias):
    """Agrega días hábiles (excluye fines de semana)"""
    fecha_resultado = fecha
    dias_agregados = 0
    
    while dias_agregados < dias:
        fecha_resultado += timedelta(days=1)
        # Si es día hábil (lunes a viernes)
        if fecha_resultado.weekday() < 5:
            dias_agregados += 1
    
    return fecha_resultado

def tiempo_restante_hasta(fecha_objetivo):
    """Calcula tiempo restante hasta una fecha"""
    ahora = datetime.now()
    diferencia = fecha_objetivo - ahora
    
    if diferencia.total_seconds() <= 0:
        return "La fecha ya pasó"
    
    dias = diferencia.days
    horas, resto = divmod(diferencia.seconds, 3600)
    minutos, segundos = divmod(resto, 60)
    
    return f"{dias} días, {horas} horas, {minutos} minutos, {segundos} segundos"

# Ejemplos prácticos
hoy = datetime.now()
navidad = datetime(2024, 12, 25)

print(f"Días hasta navidad: {calcular_dias_entre(hoy, navidad)}")

fecha_reunion = datetime(2024, 1, 15)  # Un martes
fecha_entrega = agregar_dias_habiles(fecha_reunion, 5)
print(f"Fecha de entrega: {fecha_entrega.strftime('%Y-%m-%d')}")

print(f"Tiempo hasta fin de año: {tiempo_restante_hasta(datetime(2024, 12, 31))}")
```

## 2.4 Zonas Horarias con `pytz` (librería externa común)

```python
# Requiere: pip install pytz
from datetime import datetime
import pytz

# Trabajar con zonas horarias
utc = pytz.UTC
madrid = pytz.timezone('Europe/Madrid')
nueva_york = pytz.timezone('America/New_York')

# Localizar fechas
ahora_utc = datetime.now(utc)
ahora_madrid = datetime.now(madrid)
ahora_ny = datetime.now(nueva_york)

print(f"UTC: {ahora_utc.strftime('%Y-%m-%d %H:%M:%S %Z')}")
print(f"Madrid: {ahora_madrid.strftime('%Y-%m-%d %H:%M:%S %Z')}")
print(f"NY: {ahora_ny.strftime('%Y-%m-%d %H:%M:%S %Z')}")

# Conversión entre zonas
reunion_madrid = madrid.localize(datetime(2024, 6, 15, 14, 0))
reunion_ny = reunion_madrid.astimezone(nueva_york)
print(f"Reunión en Madrid: {reunion_madrid}")
print(f"Reunión en NY: {reunion_ny}")
```

---

# **3. Matemáticas y Estadísticas - Módulos Especializados**

## 3.1 `math` - Funciones Matemáticas Fundamentales

```python
import math

# Constantes fundamentales
print(f"Pi: {math.pi}")
print(f"Euler: {math.e}")
print(f"Tau: {math.tau}")  # 2*pi
print(f"Infinito: {math.inf}")

# Funciones trigonométricas
angulo = math.radians(45)  # Grados a radianes
print(f"sin(45°): {math.sin(angulo):.3f}")
print(f"cos(45°): {math.cos(angulo):.3f}")
print(f"tan(45°): {math.tan(angulo):.3f}")

# Funciones exponenciales y logarítmicas
print(f"e^2: {math.exp(2):.3f}")
print(f"log(100): {math.log(100, 10):.3f}")  # log base 10
print(f"ln(100): {math.log(100):.3f}")       # log natural

# Funciones de potencia y raíz
print(f"2^8: {math.pow(2, 8)}")
print(f"√25: {math.sqrt(25)}")
print(f"∛27: {math.pow(27, 1/3)}")

# Funciones especializadas
print(f"Factorial de 5: {math.factorial(5)}")
print(f"MCD de 48 y 18: {math.gcd(48, 18)}")
print(f"Combinaciones C(10,3): {math.comb(10, 3)}")

# Aplicaciones prácticas
def calcular_area_circulo(radio):
    return math.pi * math.pow(radio, 2)

def calcular_hipotenusa(a, b):
    return math.sqrt(math.pow(a, 2) + math.pow(b, 2))

def distancia_entre_puntos(x1, y1, x2, y2):
    return math.sqrt(math.pow(x2 - x1, 2) + math.pow(y2 - y1, 2))

# Ejemplos
print(f"Área círculo r=5: {calcular_area_circulo(5):.2f}")
print(f"Hipotenusa 3-4: {calcular_hipotenusa(3, 4):.2f}")
print(f"Distancia (0,0)-(3,4): {distancia_entre_puntos(0, 0, 3, 4):.2f}")
```

## 3.2 `statistics` - Análisis Estadístico Básico

```python
import statistics as stats
from statistics import mean, median, mode, stdev, variance

datos = [23, 45, 67, 23, 89, 34, 23, 56, 78, 45]

# Medidas de tendencia central
print(f"Media: {mean(datos):.2f}")
print(f"Mediana: {median(datos)}")
print(f"Moda: {mode(datos)}")

# Medidas de dispersión
print(f"Varianza: {variance(datos):.2f}")
print(f"Desviación estándar: {stdev(datos):.2f}")
print(f"Rango: {max(datos) - min(datos)}")

# Cuantiles
print(f"Cuartil inferior: {stats.quantiles(datos, n=4)[0]}")
print(f"Mediana (Q2): {stats.quantiles(datos, n=4)[1]}")
print(f"Cuartil superior: {stats.quantiles(datos, n=4)[2]}")

# Funciones avanzadas
def analizar_dataset(datos):
    """Análisis completo de un dataset"""
    analisis = {
        'n': len(datos),
        'media': mean(datos),
        'mediana': median(datos),
        'moda': mode(datos),
        'varianza': variance(datos),
        'desviacion_estandar': stdev(datos),
        'minimo': min(datos),
        'maximo': max(datos),
        'rango': max(datos) - min(datos)
    }
    
    try:
        analisis['moda_multiple'] = stats.multimode(datos)
    except:
        analisis['moda_multiple'] = [analisis['moda']]
    
    return analisis

# Ejemplo con datos reales
temperaturas = [22.5, 23.1, 24.8, 22.5, 25.3, 23.1, 22.5, 26.0, 23.1]
resultado = analizar_dataset(temperaturas)

for clave, valor in resultado.items():
    print(f"{clave}: {valor}")
```

## 3.3 `random` - Generación de Números Aleatorios

```python
import random
import secrets  # Para criptografía

# Generación básica
print(f"Random 0-1: {random.random()}")
print(f"Random entero 1-100: {random.randint(1, 100)}")
print(f"Random float en rango: {random.uniform(10.5, 20.5):.2f}")

# Selecciones aleatorias
colores = ['rojo', 'verde', 'azul', 'amarillo', 'naranja']
print(f"Color aleatorio: {random.choice(colores)}")
print(f"Muestra de 3 colores: {random.sample(colores, 3)}")
print(f"Mezcla de colores: {random.shuffle(colores)} -> {colores}")

# Distribuciones probabilísticas
print(f"Distribución normal: {random.gauss(0, 1):.3f}")  # media=0, desv=1

# Semilla para reproducibilidad
random.seed(42)  # Resultados reproducibles
print(f"Con semilla: {[random.randint(1, 10) for _ in range(3)]}")

# Para seguridad/cryptografía (Python 3.6+)
print(f"Token seguro: {secrets.token_hex(16)}")
print(f"Elección segura: {secrets.choice(colores)}")

# Aplicaciones prácticas
def generar_contrasena(longitud=12):
    """Genera contraseña segura"""
    caracteres = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*"
    return ''.join(secrets.choice(caracteres) for _ in range(longitud))

def simular_lanzamiento_dado(veces=1000):
    """Simula lanzamientos de dado y calcula frecuencias"""
    resultados = [random.randint(1, 6) for _ in range(veces)]
    frecuencias = {i: resultados.count(i) for i in range(1, 7)}
    return frecuencias

# Ejemplos
print(f"Contraseña: {generar_contrasena()}")
print(f"Frecuencias dado: {simular_lanzamiento_dado(100)}")
```

---

# **4. Sistema de Archivos - `pathlib` Moderno**

## 4.1 Fundamentos de `pathlib`

```python
from pathlib import Path

# Creación de objetos Path
ruta_actual = Path()  # Directorio actual
ruta_absoluta = Path.home() / "Documents" / "proyecto"  # Concatenación con /
ruta_relativa = Path("src/utils/textos.py")

# Propiedades y métodos básicos
print(f"Nombre: {ruta_absoluta.name}")
print(f"Padre: {ruta_absoluta.parent}")
print(f"Sufijo: {ruta_absoluta.suffix}")
print(f"Stem: {ruta_absoluta.stem}")
print(f"¿Existe?: {ruta_absoluta.exists()}")
print(f"¿Es archivo?: {ruta_absoluta.is_file()}")
print(f"¿Es directorio?: {ruta_absoluta.is_dir()}")
```

## 4.2 Operaciones de Archivo con `pathlib`

```python
from pathlib import Path

def gestionar_archivos_proyecto():
    """Demostración de operaciones de archivo con pathlib"""
    
    # Crear estructura de directorios
    base_dir = Path("mi_proyecto")
    (base_dir / "src" / "utils").mkdir(parents=True, exist_ok=True)
    (base_dir / "tests").mkdir(exist_ok=True)
    (base_dir / "data" / "raw").mkdir(parents=True, exist_ok=True)
    
    # Crear archivos de configuración
    config_content = """
[settings]
version = 1.0
author = Tu Nombre
debug = true
"""
    (base_dir / "config.ini").write_text(config_content.strip())
    
    # Crear archivo Python de ejemplo
    utils_content = '''
"""Módulo de utilidades"""

def saludar(nombre):
    return f"Hola {nombre}!"

if __name__ == "__main__":
    print(saludar("Mundo"))
'''
    (base_dir / "src" / "utils" / "__init__.py").write_text(utils_content.strip())
    
    # Listar contenido
    print("Estructura creada:")
    for archivo in base_dir.rglob("*"):
        indentacion = "  " * len(archivo.relative_to(base_dir).parts)
        print(f"{indentacion}{archivo.name}")
    
    return base_dir

def buscar_archivos(extension, directorio="."):
    """Busca archivos por extensión"""
    directorio_path = Path(directorio)
    return list(directorio_path.rglob(f"*.{extension}"))

# Ejecutar demostración
proyecto = gestionar_archivos_proyecto()

# Buscar archivos Python
archivos_py = buscar_archivos("py", proyecto)
print(f"\nArchivos Python encontrados: {[p.name for p in archivos_py]}")
```

## 4.3 `os` y `shutil` - Operaciones del Sistema

```python
import os
import shutil
from pathlib import Path

# Información del sistema
print(f"Directorio actual: {os.getcwd()}")
print(f"Usuario: {os.getenv('USER', 'Desconocido')}")
print(f"Variables de entorno PATH: {os.getenv('PATH', '').split(':')[:3]}")

# Operaciones con archivos usando os
def demostrar_operaciones_os():
    """Demuestra operaciones del sistema operativo"""
    
    # Crear directorio temporal
    temp_dir = Path("temp_demo")
    temp_dir.mkdir(exist_ok=True)
    
    # Crear archivos de prueba
    archivos = ["archivo1.txt", "archivo2.log", "datos.csv"]
    for archivo in archivos:
        (temp_dir / archivo).write_text(f"Contenido de {archivo}")
    
    # Listar con os
    print("Contenido del directorio:")
    for elemento in os.listdir(temp_dir):
        ruta_completa = temp_dir / elemento
        tamaño = os.path.getsize(ruta_completa)
        print(f"  {elemento} ({tamaño} bytes)")
    
    # Estadísticas de archivo
    stats = os.stat(temp_dir / "archivo1.txt")
    print(f"\nEstadísticas archivo1.txt:")
    print(f"  Tamaño: {stats.st_size} bytes")
    print(f"  Creado: {stats.st_ctime}")
    print(f"  Modificado: {stats.st_mtime}")
    
    # Copiar archivos con shutil
    shutil.copy(temp_dir / "archivo1.txt", temp_dir / "archivo1_backup.txt")
    
    # Limpiar
    shutil.rmtree(temp_dir)
    print(f"\nDirectorio {temp_dir} eliminado")

demostrar_operaciones_os()
```

---

# **5. Serialización - `json` y `pickle`**

## 5.1 Trabajo Avanzado con JSON

```python
import json
from pathlib import Path
from datetime import datetime
from typing import Any, Dict

class JSONEncoderPersonalizado(json.JSONEncoder):
    """Encoder personalizado para tipos complejos"""
    
    def default(self, obj: Any) -> Any:
        if isinstance(obj, datetime):
            return obj.isoformat()
        elif isinstance(obj, Path):
            return str(obj)
        elif hasattr(obj, '__dict__'):
            return obj.__dict__
        return super().default(obj)

def guardar_configuracion(config: Dict, ruta: Path) -> None:
    """Guarda configuración en archivo JSON"""
    try:
        with open(ruta, 'w', encoding='utf-8') as archivo:
            json.dump(config, archivo, indent=2, ensure_ascii=False, 
                     cls=JSONEncoderPersonalizado)
        print(f"Configuración guardada en {ruta}")
    except (IOError, TypeError) as e:
        print(f"Error guardando configuración: {e}")

def cargar_configuracion(ruta: Path) -> Dict:
    """Carga configuración desde archivo JSON"""
    try:
        with open(ruta, 'r', encoding='utf-8') as archivo:
            return json.load(archivo)
    except (FileNotFoundError, json.JSONDecodeError) as e:
        print(f"Error cargando configuración: {e}")
        return {}

# Ejemplo de uso
configuracion = {
    "proyecto": {
        "nombre": "Mi Proyecto",
        "version": "1.0.0",
        "fecha_creacion": datetime.now()
    },
    "database": {
        "host": "localhost",
        "port": 5432,
        "usuario": "admin"
    },
    "rutas": {
        "datos": Path("data/raw"),
        "modelos": Path("models"),
        "logs": Path("logs")
    }
}

# Guardar y cargar configuración
ruta_config = Path("config.json")
guardar_configuracion(configuracion, ruta_config)

config_cargada = cargar_configuracion(ruta_config)
print("Configuración cargada:")
print(json.dumps(config_cargada, indent=2))
```

## 5.2 `pickle` para Objetos Python

```python
import pickle
from pathlib import Path

class ResultadoAnalisis:
    """Clase para demostrar serialización con pickle"""
    
    def __init__(self, datos, metricas, timestamp):
        self.datos = datos
        self.metricas = metricas
        self.timestamp = timestamp
        self.version = "1.0"
    
    def __repr__(self):
        return f"ResultadoAnalisis({len(self.datos)} datos, {len(self.metricas)} métricas)"

def demostrar_pickle():
    """Demuestra serialización con pickle"""
    
    # Crear objeto complejo
    resultado = ResultadoAnalisis(
        datos=list(range(100)),
        metricas={"precisión": 0.95, "recall": 0.87, "f1": 0.91},
        timestamp=datetime.now()
    )
    
    # Serializar con pickle
    ruta_pickle = Path("resultado.pkl")
    
    try:
        with open(ruta_pickle, 'wb') as archivo:
            pickle.dump(resultado, archivo)
        print(f"Objeto guardado en {ruta_pickle}")
        
        # Deserializar
        with open(ruta_pickle, 'rb') as archivo:
            resultado_cargado = pickle.load(archivo)
        
        print(f"Objeto cargado: {resultado_cargado}")
        print(f"Métricas: {resultado_cargado.metricas}")
        
    except (pickle.PickleError, IOError) as e:
        print(f"Error con pickle: {e}")
    
    finally:
        # Limpiar
        if ruta_pickle.exists():
            ruta_pickle.unlink()

# Advertencia de seguridad con pickle
print("⚠️  ADVERTENCIA: pickle puede ejecutar código arbitrario.")
print("   Solo usar con datos de confianza.\n")

demostrar_pickle()
```

---

# **6. Módulos Avanzados de la Biblioteca Estándar**

## 6.1 `collections` - Estructuras de Datos Especializadas

```python
from collections import Counter, defaultdict, deque, namedtuple
from typing import List, Dict

# Counter - conteo eficiente
def analizar_texto(texto: str) -> Dict:
    """Analiza texto usando Counter"""
    palabras = texto.lower().split()
    contador = Counter(palabras)
    
    print("Palabras más comunes:")
    for palabra, cuenta in contador.most_common(5):
        print(f"  {palabra}: {cuenta}")
    
    return contador

# defaultdict - diccionarios con valores por defecto
def agrupar_por_longitud(palabras: List[str]) -> Dict[int, List[str]]:
    """Agrupa palabras por longitud"""
    grupos = defaultdict(list)
    for palabra in palabras:
        grupos[len(palabra)].append(palabra)
    return dict(grupos)

# deque - cola de doble extremo eficiente
def procesar_tareas(tareas: List[str], max_tiempo: int = 5):
    """Simula procesamiento de tareas con deque"""
    cola = deque(tareas)
    tiempo = 0
    
    while cola and tiempo < max_tiempo:
        tarea_actual = cola.popleft()
        print(f"Procesando: {tarea_actual}")
        tiempo += 1
        
        # Simular nueva tarea ocasionalmente
        if tiempo % 2 == 0 and cola:
            nueva_tarea = f"tarea_urgente_{tiempo}"
            cola.appendleft(nueva_tarea)
            print(f"  → Nueva tarea urgente: {nueva_tarea}")
    
    print(f"Tareas pendientes: {list(cola)}")

# namedtuple - tuplas con nombres
Coordenada = namedtuple('Coordenada', ['x', 'y', 'z'])

def calcular_distancia(p1: Coordenada, p2: Coordenada) -> float:
    """Calcula distancia entre coordenadas"""
    return ((p2.x - p1.x)**2 + (p2.y - p1.y)**2 + (p2.z - p1.z)**2) ** 0.5

# Ejemplos de uso
texto = "python es genial y python es poderoso"
analizar_texto(texto)

palabras = ["sol", "luna", "estrella", "mar", "cielo", "nube"]
print(f"Agrupadas por longitud: {agrupar_por_longitud(palabras)}")

tareas = ["tarea1", "tarea2", "tarea3", "tarea4"]
procesar_tareas(tareas)

punto1 = Coordenada(1, 2, 3)
punto2 = Coordenada(4, 6, 8)
print(f"Distancia entre puntos: {calcular_distancia(punto1, punto2):.2f}")
```

## 6.2 `itertools` - Iteradores Avanzados

```python
import itertools
from typing import List, Any

def demostrar_itertools():
    """Demuestra las poderosas herramientas de itertools"""
    
    # Combinatorias
    letras = ['A', 'B', 'C']
    
    print("Combinaciones de 2:")
    for combo in itertools.combinations(letras, 2):
        print(f"  {combo}")
    
    print("\nPermutaciones:")
    for perm in itertools.permutations(letras, 2):
        print(f"  {perm}")
    
    print("\nProducto cartesiano:")
    for prod in itertools.product([1, 2], ['a', 'b']):
        print(f"  {prod}")
    
    # Iteradores infinitos
    print("\nContador:")
    contador = itertools.count(start=10, step=2)
    for i, num in enumerate(contador):
        if i >= 5:
            break
        print(f"  {num}")
    
    # Agrupamiento
    datos = [1, 1, 2, 3, 3, 3, 4, 4, 5]
    print("\nAgrupado:")
    for clave, grupo in itertools.groupby(datos):
        print(f"  {clave}: {list(grupo)}")
    
    # Cadena de iteradores
    lista1 = [1, 2, 3]
    lista2 = [4, 5, 6]
    lista3 = [7, 8, 9]
    
    print("\nCadenas:")
    for elemento in itertools.chain(lista1, lista2, lista3):
        print(f"  {elemento}")

def generar_combinaciones_avanzadas(elementos: List[Any], n: int):
    """Genera combinaciones avanzadas para análisis"""
    resultados = {
        'combinaciones': list(itertools.combinations(elementos, n)),
        'combinaciones_con_repeticion': list(itertools.combinations_with_replacement(elementos, n)),
        'permutaciones': list(itertools.permutations(elementos, n)),
        'productos': list(itertools.product(elementos, repeat=n))
    }
    return resultados

# Ejemplo de uso
elementos = ['X', 'Y', 'Z']
combinaciones = generar_combinaciones_avanzadas(elementos, 2)

for tipo, valores in combinaciones.items():
    print(f"\n{tipo}:")
    for valor in valores[:5]:  # Mostrar solo primeros 5
        print(f"  {valor}")

demostrar_itertools()
```

---

# **7. Guía de Elección: ¿Cuándo Usar Qué Módulo?**

## Árbol de Decisión para Selección de Módulos

```
¿Necesitas trabajar con fechas/tiempos?
├── ¿Fechas completas con zona horaria? → datetime + pytz
├── ¿Solo medición de tiempo? → time
└── ¿Operaciones temporales simples? → datetime

¿Necesitas operaciones matemáticas?
├── ¿Funciones matemáticas básicas? → math
├── ¿Análisis estadístico? → statistics  
└── ¿Números aleatorios? → random (secrets para crypto)

¿Necesitas manejar archivos/directorios?
├── ¿Rutas y operaciones básicas? → pathlib (moderno)
├── ¿Operaciones del sistema? → os, shutil
└── ¿Patrones de archivos? → glob

¿Necesitas estructuras de datos?
├── ¿Conteo eficiente? → collections.Counter
├── ¿Diccionarios con default? → collections.defaultdict
└── ¿Colas eficientes? → collections.deque

¿Necesitas serialización?
├── ¿Interoperabilidad? → json
├── ¿Objetos Python? → pickle (con cuidado)
└── ¿Tipos complejos? → json con encoder personalizado
```

## Tabla de Módulos por Categoría

| Categoría | Módulos Principales | Casos de Uso |
|-----------|---------------------|--------------|
| **Tiempo** | `datetime`, `time`, `calendar` | Fechas, horarios, zonas horarias |
| **Matemáticas** | `math`, `statistics`, `random` | Cálculos, análisis, simulaciones |
| **Sistema** | `os`, `pathlib`, `shutil` | Archivos, directorios, rutas |
| **Datos** | `collections`, `itertools` | Estructuras especializadas, iteradores |
| **Serialización** | `json`, `pickle` | Persistencia, intercambio de datos |
| **Funcional** | `functools`, `operator` | Programación funcional |
| **Texto** | `re`, `string` | Expresiones regulares, manipulación |

---

