# **Manejo de Strings en Python - Guía Completa Intermedio/Avanzado**

## 0. Visión General: El Poder de los Strings en Python

Los strings en Python son mucho más que secuencias de caracteres:

- **Inmutables**: Seguridad en hilos y hashable para diccionarios
- **Unicode nativo**: Soporte completo para internacionalización
- **Rico conjunto de métodos**: Más de 40 métodos integrados
- **Flexibilidad de representación**: Múltiples formas de crear y formatear
- **Eficiencia optimizada**: Técnicas avanzadas para procesamiento masivo

---

# **1. Creación y Representación - Más Allá de lo Básico**

## 1.1 Múltiples Formas de Creación

```python
# Comillas simples y dobles
s1 = 'Python'
s2 = "Programming"

# Triples comillas para multilínea
s3 = """Línea 1
Línea 2
Línea 3"""

# Raw strings (ignoran escapes)
ruta = r"C:\Users\nombre\archivo.txt"
regex = r"\d+\.\d+"

# Bytes vs Strings
cadena_unicode = "café"  # str
cadena_bytes = b"cafe"   # bytes

# Strings con formato incrustado
nombre = "Ana"
saludo = f"Hola {nombre}"
```

## 1.2 Caracteres Especiales y Escapado

```python
# Secuencias de escape comunes
print("Salto\nde\nlínea")
print("Tabulación\t->\ttexto")
print("Comillas: \"texto entre comillas\"")
print("Barra invertida: \\")

# Unicode escapes
print("Euro: \u20AC")      # €
print("Corazón: \u2665")   # ♥
print("Emoji: \U0001F600") # 😀

# Raw strings para evitar escapes
regex_pattern = r"\d{3}-\d{2}-\d{4}"
windows_path = r"C:\Users\Public\Documents"
```

## 1.3 Strings Multilínea sin Escapado

```python
# Usando triples comillas con comillas internas
html_content = """
<div class="container">
    <h1 id="title">Título</h1>
    <p class="text">Texto con "comillas" internas</p>
</div>
"""

sql_query = """
SELECT * FROM users 
WHERE name = "John" 
AND age > 18
"""

# Preservando formato exacto
plantilla_carta = """\
Estimado {nombre},

Le escribo para informarle sobre {asunto}.

Atentamente,
{empresa}
"""
```

---

# **2. Indexación y Slicing - Técnicas Avanzadas**

## 2.1 Indexación Positiva y Negativa

```python
texto = "Python Programming"

# Indexación positiva (0-based)
print(texto[0])    # 'P'
print(texto[7])    # 'P' (de Programming)

# Indexación negativa (desde el final)
print(texto[-1])   # 'g'
print(texto[-11])  # 'P' (de Programming)

# Índices fuera de rango
try:
    print(texto[100])
except IndexError as e:
    print(f"Error: {e}")
```

## 2.2 Slicing Completo

```python
texto = "Python Programming"

# Sintaxis: [inicio:fin:paso]
print(texto[0:6])      # "Python"
print(texto[7:18])     # "Programming"
print(texto[:6])       # "Python" (desde inicio)
print(texto[7:])       # "Programming" (hasta final)
print(texto[::2])      # "Pto rgamn" (cada 2 caracteres)
print(texto[::-1])     # "gnimmargorP nohtyP" (invertido)

# Slicing con índices negativos
print(texto[-11:])     # "Programming"
print(texto[:-12])     # "Python"
print(texto[-18:-12])  # "Python"

# Patrones útiles
def invertir_string(s):
    """Invierte un string usando slicing"""
    return s[::-1]

def obtener_ultimos_n(s, n):
    """Obtiene los últimos n caracteres"""
    return s[-n:] if len(s) >= n else s

def cada_n_caracteres(s, n):
    """Obtiene cada n-ésimo carácter"""
    return s[::n]

# Ejemplos
print(invertir_string("Python"))           # "nohtyP"
print(obtener_ultimos_n("Programming", 4)) # "ming"
print(cada_n_caracteres("ABCDEFGH", 2))    # "ACEG"
```

## 2.3 Slicing con Asignación (para mutables)

```python
# Los strings son inmutables, pero podemos simular modificaciones
def reemplazar_substring(texto, inicio, fin, nuevo):
    """Reemplaza una porción del string"""
    return texto[:inicio] + nuevo + texto[fin:]

original = "Python Programming"
modificado = reemplazar_substring(original, 7, 18, "Coding")
print(modificado)  # "Python Coding"
```

---

# **3. Métodos Nativos - Catálogo Completo**

## 3.1 Transformación de Mayúsculas/Minúsculas

```python
texto = "python Programming"

print(texto.upper())        # "PYTHON PROGRAMMING"
print(texto.lower())        # "python programming"
print(texto.capitalize())   # "Python programming"
print(texto.title())        # "Python Programming"
print(texto.swapcase())     # "PYTHON pROGRAMMING"

# Casos especiales
print("ß".upper())          # "SS" (German sharp S)
print("İ".lower())          # "i" (Turkish dotted I)

# Para comparaciones case-insensitive
def igual_insensitivo(a, b):
    return a.lower() == b.lower()

print(igual_insensitivo("Python", "PYTHON"))  # True
```

## 3.2 Eliminación de Espacios y Caracteres

```python
# Espacios en blanco
texto = "   Python Programming   "
print(f"'{texto.strip()}'")     # 'Python Programming'
print(f"'{texto.lstrip()}'")    # 'Python Programming   '
print(f"'{texto.rstrip()}'")    # '   Python Programming'

# Caracteres específicos
url = "https://example.com/"
print(url.strip("htps:/"))      # "example.com"

# Múltiples caracteres
codigo = ">>> print('hello') <<<"
print(codigo.strip('> <'))      # "print('hello')"
```

## 3.3 Búsqueda y Conteo

```python
texto = "Python programming with Python"

# Búsqueda simple
print(texto.find("Python"))     # 0
print(texto.rfind("Python"))    # 23
print(texto.find("Java"))       # -1 (no encontrado)

# Búsqueda con índices
print(texto.index("Python"))    # 0
try:
    print(texto.index("Java"))  # ValueError
except ValueError:
    print("No encontrado")

# Conteo de ocurrencias
print(texto.count("Python"))    # 2
print(texto.count("p"))         # 1 (case-sensitive)
print(texto.count("P"))         # 2

# Búsqueda con rangos
print(texto.find("Python", 10)) # Busca desde posición 10
```

## 3.4 Reemplazo y Transformación

```python
texto = "Python programming"

# Reemplazo simple
print(texto.replace("Python", "Java"))  # "Java programming"

# Reemplazo con límite
print(texto.replace("n", "N", 1))       # "PythoN programming"

# Reemplazo múltiple (patrón común)
def reemplazo_multiple(texto, reemplazos):
    for viejo, nuevo in reemplazos.items():
        texto = texto.replace(viejo, nuevo)
    return texto

cambios = {"Python": "Java", "programming": "coding"}
print(reemplazo_multiple(texto, cambios))  # "Java coding"
```

---

# **4. División y Unión - Patrones Avanzados**

## 4.1 División con `split()`

```python
# División básica
csv = "a,b,c,d,e"
print(csv.split(","))        # ['a', 'b', 'c', 'd', 'e']

# Límite de divisiones
texto = "uno dos tres cuatro"
print(texto.split(" ", 2))   # ['uno', 'dos', 'tres cuatro']

# División por whitespace (por defecto)
frase = "Python   es    genial"
print(frase.split())         # ['Python', 'es', 'genial']

# División desde la derecha con rsplit
path = "home/user/documents/file.txt"
print(path.rsplit("/", 1))   # ['home/user/documents', 'file.txt']
```

## 4.2 Unión con `join()` - Alta Eficiencia

```python
# Unión básica
palabras = ["Python", "es", "genial"]
print(" ".join(palabras))    # "Python es genial"

# Join con diferentes separadores
print(",".join(palabras))    # "Python,es,genial"
print("".join(palabras))     # "Pythonesgenial"
print("-".join(palabras))    # "Python-es-genial"

# Join es más eficiente que concatenación
def construir_string_ineficiente(palabras):
    resultado = ""
    for palabra in palabras:
        resultado += palabra + " "  # ❌ Ineficiente
    return resultado.strip()

def construir_string_eficiente(palabras):
    return " ".join(palabras)      # ✅ Eficiente

# Procesamiento de texto completo
def procesar_parrafo(parrafo):
    """Divide, procesa y vuelve a unir un párrafo"""
    palabras = parrafo.split()
    palabras_ordenadas = sorted(palabras, key=str.lower)
    return " ".join(palabras_ordenadas)

texto = "Python es un lenguaje de programación poderoso"
print(procesar_parrafo(texto))
# "de es lenguaje poderoso programación Python un"
```

## 4.3 División con `partition()` y `rpartition()`

```python
# partition() - divide en 3 partes
email = "usuario@dominio.com"
usuario, separador, dominio = email.partition("@")
print(f"Usuario: {usuario}, Dominio: {dominio}")

# rpartition() - divide desde la derecha
path = "/home/user/documents/file.txt"
directorio, sep, archivo = path.rpartition("/")
print(f"Directorio: {directorio}, Archivo: {archivo}")

# Para extraer extensiones
filename = "documento.backup.pdf"
nombre, sep, extension = filename.rpartition(".")
print(f"Nombre: {nombre}, Extensión: {extension}")

# Múltiples divisiones con partition
def parsear_ruta_completa(ruta):
    """Parsea una ruta en sus componentes"""
    protocolo, sep, resto = ruta.partition("://")
    dominio, sep, ruta_final = resto.partition("/")
    return protocolo, dominio, "/" + ruta_final

url = "https://example.com/path/to/resource"
print(parsear_ruta_completa(url))
```

---

# **5. Validación e Inspección - Métodos Booleanos**

## 5.1 Validación de Contenido

```python
# Métodos de verificación
print("123".isdigit())       # True
print("abc".isalpha())       # True  
print("abc123".isalnum())    # True
print("python".islower())    # True
print("PYTHON".isupper())    # True
print("Python".istitle())    # True
print("   ".isspace())       # True

# Validación de strings vacíos o whitespace
def es_string_valido(texto):
    return texto and not texto.isspace()

print(es_string_valido(""))          # False
print(es_string_valido("   "))       # False
print(es_string_valido("Python"))    # True

# Validación de formato específico
def es_identificador_valido(identificador):
    """Valida si es un identificador Python válido"""
    return identificador.isidentifier()

print(es_identificador_valido("variable1"))  # True
print(es_identificador_valido("1variable"))  # False
```

## 5.2 Prefijos y Sufijos

```python
texto = "Python programming"

print(texto.startswith("Python"))    # True
print(texto.endswith("programming")) # True
print(texto.startswith("py"))        # False (case-sensitive)

# Múltiples opciones
archivos = ["file.txt", "image.jpg", "document.pdf"]
for archivo in archivos:
    if archivo.endswith((".txt", ".pdf")):
        print(f"Documento: {archivo}")

# Validación de username
def es_username_valido(username):
    """Valida un username según reglas específicas"""
    return (
        len(username) >= 3 and
        len(username) <= 15 and
        username.isalnum() and
        not username[0].isdigit() and
        username.islower()
    )

print(es_username_valido("user123"))     # True
print(es_username_valido("123user"))     # False
print(es_username_valido("AdminUser"))   # False
```

---

# **6. Operadores con Strings - Comportamientos Avanzados**

## 6.1 Concatenación y Repetición

```python
# Concatenación básica
s1 = "Hello"
s2 = "World"
print(s1 + " " + s2)  # "Hello World"

# Repetición
print("Ha" * 3)       # "HaHaHa"
print("-" * 20)       # "--------------------"

# Concatenación con otros tipos (requiere conversión)
numero = 42
#print("El número es " + numero)  # TypeError
print("El número es " + str(numero))  # Correcto

# Concatenación en bucle (ineficiente)
def construir_string_ineficiente(n):
    resultado = ""
    for i in range(n):
        resultado += str(i)  # ❌ O(n²) en el peor caso
    return resultado

# Concatenación eficiente con join
def construir_string_eficiente(n):
    elementos = [str(i) for i in range(n)]
    return "".join(elementos)  # ✅ O(n)
```

## 6.2 Pertenencia y Comparación

```python
# Operador in
texto = "Python programming"
print("Python" in texto)    # True
print("java" in texto)      # False
print("python" in texto)    # False (case-sensitive)

# Comparación lexicográfica
print("apple" < "banana")   # True
print("apple" < "Apple")    # False (A=65, a=97 en ASCII)

# Ordenamiento personalizado
palabras = ["banana", "Apple", "cherry", "apple"]
print(sorted(palabras))     # ['Apple', 'apple', 'banana', 'cherry']

# Ordenamiento case-insensitive
print(sorted(palabras, key=str.lower))  # ['Apple', 'apple', 'banana', 'cherry']

# Ordenamiento por múltiples criterios
def ordenar_por_longitud_y_alfabetico(palabras):
    return sorted(palabras, key=lambda x: (len(x), x.lower()))

palabras = ["python", "Java", "C", "c++", "JavaScript"]
print(ordenar_por_longitud_y_alfabetico(palabras))
# ['C', 'c++', 'Java', 'python', 'JavaScript']
```

---

# **7. Formateo Avanzado - f-strings y Más**

## 7.1 f-strings (Python 3.6+)

```python
nombre = "Ana"
edad = 25
altura = 1.75

# Básico
print(f"Hola {nombre}")  # "Hola Ana"

# Expresiones
print(f"Edad el próximo año: {edad + 1}")  # "Edad el próximo año: 26"

# Llamadas a métodos
print(f"Nombre en mayúsculas: {nombre.upper()}")  # "Nombre en mayúsculas: ANA"

# Formateo numérico
print(f"Altura: {altura:.2f} metros")      # "Altura: 1.75 metros"
print(f"Porcentaje: {0.256:.1%}")          # "Porcentaje: 25.6%"
print(f"Número grande: {1000000:,}")       # "Número grande: 1,000,000"
print(f"Hexadecimal: {255:#x}")            # "Hexadecimal: 0xff"

# Alineación
print(f"|{nombre:<10}|")   # "|Ana       |" (izquierda)
print(f"|{nombre:>10}|")   # "|       Ana|" (derecha)  
print(f"|{nombre:^10}|")   # "|   Ana    |" (centrado)

# Relleno personalizado
print(f"|{nombre:*^10}|")  # "|***Ana****|"
```

## 7.2 Tablas y Columnas con f-strings

```python
def crear_tabla(datos):
    """Crea una tabla formateada con f-strings"""
    # Encabezados
    headers = ["Nombre", "Edad", "Salario", "Departamento"]
    
    # Anchos de columna
    anchos = [15, 8, 12, 15]
    
    # Línea separadora
    separador = "+" + "+".join("-" * ancho for ancho in anchos) + "+"
    
    # Construir tabla
    tabla = [separador]
    
    # Encabezados
    fila_headers = "|" + "|".join(f"{h:^{a}}" for h, a in zip(headers, anchos)) + "|"
    tabla.extend([fila_headers, separador])
    
    # Datos
    for nombre, edad, salario, depto in datos:
        fila = "|" + "|".join([
            f"{nombre:<{anchos[0]}}",
            f"{edad:>{anchos[1]}}",
            f"${salario:>{anchos[2]-1},}",
            f"{depto:<{anchos[3]}}"
        ]) + "|"
        tabla.append(fila)
    
    tabla.append(separador)
    return "\n".join(tabla)

# Datos de ejemplo
empleados = [
    ("Ana García", 28, 45000, "Ventas"),
    ("Carlos López", 35, 62000, "IT"),
    ("María Rodríguez", 42, 58000, "RH")
]

print(crear_tabla(empleados))
```

## 7.3 `str.format()` - Alternativa Potente

```python
# Posicional
print("{} + {} = {}".format(2, 3, 5))  # "2 + 3 = 5"

# Nombrado
print("Hola {nombre}, tienes {edad} años".format(nombre="Ana", edad=25))

# Acceso a atributos
from collections import namedtuple
Persona = namedtuple('Persona', ['nombre', 'edad'])
p = Persona("Carlos", 30)
print("Persona: {p.nombre}, {p.edad}".format(p=p))

# Formateo avanzado
print("{:.2f}".format(3.14159))        # "3.14"
print("{:>10}".format("texto"))        # "     texto"
print("{:0>5}".format(42))             # "00042"
```

---

# **8. Métodos Avanzados - Transformaciones Complejas**

## 8.1 Alineación y Relleno

```python
texto = "Python"

# Alineación básica
print(texto.ljust(10))      # "Python    "
print(texto.rjust(10))      # "    Python"
print(texto.center(10))     # "  Python  "

# Relleno personalizado
print(texto.ljust(10, "*")) # "Python****"
print(texto.rjust(10, "-")) # "----Python"
print(texto.center(10, "+")) # "++Python++"

# zfill para números
print("42".zfill(5))        # "00042"
print("-42".zfill(5))       # "-0042"

# Aplicación: formateo de IDs
def formatear_id(numero, longitud=8):
    return str(numero).zfill(longitud)

print(formatear_id(123))    # "00000123"
print(formatear_id(9999))   # "00009999"
```

## 8.2 `translate()` y `maketrans()` - Reemplazo Múltiple Eficiente

```python
# Reemplazo simple con translate
tabla = str.maketrans("aeiou", "12345")
texto = "hello world"
print(texto.translate(tabla))  # "h2ll4 w4rld"

# Eliminación de caracteres
tabla_eliminar = str.maketrans("", "", "aeiou")
print(texto.translate(tabla_eliminar))  # "hll wrld"

# Reemplazo complejo
def cifrar_cesar(texto, desplazamiento=3):
    """Cifrado César básico usando translate"""
    alfabeto = "abcdefghijklmnopqrstuvwxyz"
    cifrado = alfabeto[desplazamiento:] + alfabeto[:desplazamiento]
    tabla = str.maketrans(alfabeto + alfabeto.upper(), 
                         cifrado + cifrado.upper())
    return texto.translate(tabla)

def crear_cifrado_personalizado():
    """Crea un cifrado por sustitución"""
    original = "abcdefghijklmnopqrstuvwxyz"
    sustitución = "qwertyuiopasdfghjklzxcvbnm"  # Teclado QWERTY
    tabla = str.maketrans(original + original.upper(),
                         sustitución + sustitución.upper())
    return tabla

# Ejemplos
print(cifrar_cesar("hello", 3))  # "khoor"

cifrador = crear_cifrado_personalizado()
mensaje = "python"
print(mensaje.translate(cifrador))  # "bdgufr"
```

## 8.3 `partition()` vs `split()` - Casos de Uso

```python
def procesar_configuracion(linea):
    """Procesa líneas de configuración clave=valor"""
    if "=" in linea:
        clave, sep, valor = linea.partition("=")
        return clave.strip(), valor.strip()
    return None

def parsear_comando(comando):
    """Parsea comandos con opciones"""
    comando_principal, sep, opciones = comando.partition(" ")
    return comando_principal, opciones.split() if opciones else []

# Ejemplos
config = "nombre = Juan"
print(procesar_configuracion(config))  # ('nombre', 'Juan')

cmd = "copiar -r -v archivo1 archivo2"
print(parsear_comando(cmd))  # ('copiar', ['-r', '-v', 'archivo1', 'archivo2'])
```

---

# **9. Strings como Colecciones - Técnicas Funcionales**

## 9.1 Iteración y Comprensiones

```python
texto = "Python 3.9"

# Iteración básica
for caracter in texto:
    print(caracter, end=" ")
print()  # P y t h o n   3 . 9

# Comprensión de caracteres
vocales = [c for c in texto if c.lower() in "aeiou"]
print(f"Vocales: {vocales}")  # ['o']

# Filtrado con condición
alfanumericos = [c for c in texto if c.isalnum()]
print(f"Alfanuméricos: {alfanumericos}")  # ['P', 'y', 't', 'h', 'o', 'n', '3', '9']

# Transformación
invertido = [c for c in reversed(texto)]
print(f"Invertido: {''.join(invertido)}")  # "9.3 nohtyP"
```

## 9.2 Técnicas de Procesamiento Funcional

```python
from functools import reduce

texto = "python programming"

# Map para transformación
mayusculas = ''.join(map(str.upper, texto))
print(mayusculas)  # "PYTHON PROGRAMMING"

# Filter para filtrado
solo_letras = ''.join(filter(str.isalpha, texto))
print(solo_letras)  # "pythonprogramming"

# Reduce para agregación
def contar_vocales(texto):
    return reduce(
        lambda count, c: count + 1 if c.lower() in 'aeiou' else count,
        texto, 0
    )

print(f"Vocales: {contar_vocales(texto)}")  # 4
```

---

# **10. Técnicas Profesionales - Optimización y Buenas Prácticas**

## 10.1 Normalización Unicode

```python
import unicodedata

def normalizar_texto(texto, forma='NFC'):
    """
    Normaliza texto Unicode para comparaciones consistentes
    
    NFC: Composición preferida (é como único carácter)
    NFD: Descomposición (é como e + ´)
    NFKC: Composición compatible (ancho completo → medio)
    NFKD: Descomposición compatible
    """
    return unicodedata.normalize(forma, texto)

# Ejemplos con caracteres acentuados
texto1 = "café"
texto2 = "cafe\u0301"  # e + acento agudo

print(f"Texto1: {texto1}")  # café
print(f"Texto2: {texto2}")  # café (visualmente igual)
print(f"¿Iguales?: {texto1 == texto2}")  # False

texto1_norm = normalizar_texto(texto1)
texto2_norm = normalizar_texto(texto2)
print(f"¿Iguales normalizados?: {texto1_norm == texto2_norm}")  # True

# Limpieza completa
def limpiar_texto(texto):
    """Limpia y normaliza texto"""
    # Normalizar
    texto = unicodedata.normalize('NFKD', texto)
    # Remover diacríticos
    texto = ''.join(c for c in texto if not unicodedata.combining(c))
    # A minúsculas
    return texto.lower()

print(limpiar_texto("Café au Lait"))  # "cafe au lait"
```

## 10.2 String Builder Eficiente

```python
def construir_string_eficiente(*partes):
    """Construye strings grandes eficientemente"""
    return ''.join(partes)

def construir_tabla_dinamica(datos, encabezados):
    """Construye una tabla HTML eficientemente"""
    buffer = []
    
    # Encabezado
    buffer.append('<table>\n  <tr>')
    buffer.extend(f'<th>{h}</th>' for h in encabezados)
    buffer.append('</tr>\n')
    
    # Filas de datos
    for fila in datos:
        buffer.append('  <tr>')
        buffer.extend(f'<td>{celda}</td>' for celda in fila)
        buffer.append('</tr>\n')
    
    buffer.append('</table>')
    return ''.join(buffer)

# Uso
datos = [["Ana", "25"], ["Carlos", "30"]]
encabezados = ["Nombre", "Edad"]
html = construir_tabla_dinamica(datos, encabezados)
print(html)
```

## 10.3 Expresiones Regulares para Procesamiento Complejo

```python
import re

# Validación de email
def es_email_valido(email):
    patron = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return bool(re.match(patron, email))

# Extracción de información
def extraer_telefonos(texto):
    patron = r'(\+\d{1,3}[-.]?)?\(?\d{3}\)?[-.]?\d{3}[-.]?\d{4}'
    return re.findall(patron, texto)

# Limpieza de texto
def limpiar_texto_avanzado(texto):
    # Remover caracteres especiales excepto espacios y letras básicas
    texto = re.sub(r'[^a-zA-Z0-9\sáéíóúÁÉÍÓÚñÑ]', '', texto)
    # Normalizar espacios múltiples
    texto = re.sub(r'\s+', ' ', texto)
    return texto.strip()

# Ejemplos
print(es_email_valido("usuario@example.com"))  # True
print(extraer_telefonos("Llame al 555-123-4567 o al (555) 987-6543"))
print(limpiar_texto_avanzado("¡Hola! ¿Cómo   estás?"))  # "Hola Cómo estás"
```

## 10.4 Plantillas con `string.Template`

```python
from string import Template
import datetime

class PlantillaCorreo:
    def __init__(self):
        self.plantilla = Template("""
Hola $nombre,

Tu pedido #$pedido_id ha sido procesado el $fecha.

Total: $${monto}
Estado: $estado

Gracias,
El equipo de $empresa
        """.strip())
    
    def generar_correo(self, datos):
        datos['fecha'] = datetime.datetime.now().strftime('%Y-%m-%d')
        return self.plantilla.substitute(datos)

# Uso
plantilla = PlantillaCorreo()
datos_pedido = {
    'nombre': 'Ana García',
    'pedido_id': '12345',
    'monto': '99.99',
    'estado': 'Enviado',
    'empresa': 'MiTienda'
}

print(plantilla.generar_correo(datos_pedido))
```

---

# **11. Guía de Elección: ¿Cuándo Usar Qué Técnica?**

## Árbol de Decisión para Manipulación de Strings

```
¿Necesitas modificar el string?
├── ¿Pequeños cambios? → Slicing + concatenación
├── ¿Múltiples reemplazos? → translate()/replace()
└── ¿Construcción compleja? → String Builder con join()

¿Necesitas dividir el string?
├── ¿En partes fijas? → partition()/rpartition()
├── ¿En múltiples partes? → split()/rsplit()
└── ¿Por patrones complejos? → re.split()

¿Necesitas formatear?
├── ¿Variables simples? → f-strings
├── ¿Plantillas reutilizables? → string.Template
└── ¿Formato complejo? → str.format()

¿Validación/verificación?
├── ¿Patrones simples? → métodos is*
├── ¿Prefijos/sufijos? → startswith()/endswith()
└── ¿Patrones complejos? → Expresiones regulares
```

## Tabla Comparativa de Técnicas

| Operación | Método Recomendado | Razón |
|-----------|-------------------|-------|
| Concatenación múltiple | `"".join()` | Eficiencia O(n) |
| Búsqueda simple | `find()`/`index()` | Sencillez |
| Búsqueda compleja | `re.search()` | Flexibilidad |
| Reemplazo múltiple | `translate()` | Eficiencia |
| Reemplazo simple | `replace()` | Sencillez |
| División en partes | `split()` | Para múltiples elementos |
| División en 3 partes | `partition()` | Para separadores únicos |
| Formateo moderno | f-strings | Legibilidad y poder |
| Plantillas reutilizables | `string.Template` | Seguridad y reuso |

---
