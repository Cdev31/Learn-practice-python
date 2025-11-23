# **Estructuras de Control en Python - Guía Completa Intermedio/Avanzado**

## 0. Visión General: El Ecosistema de Control en Python

Python ofrece un conjunto versátil de estructuras de control que van más allá del `if/else` básico:

- **Condicionales secuenciales**: `if / elif / else`
- **Expresiones condicionales**: Operador ternario
- **Patrones de diseño**: Diccionarios de funciones (alternativa a switch)
- **Coincidencia de patrones**: `match-case` (Python 3.10+)
- **Técnicas funcionales**: Filtrado con comprensiones
- **Optimizaciones**: Short-circuit evaluation y early returns

---

# **1. Condicionales `if`, `elif`, `else` - Dominio Completo**

## 1.1 Anatomía y Flujo de Ejecución

```python
if condicion_primaria:
    # Bloque A - ejecuta SI condicion_primaria es True
    resultado = "caso_primario"
elif condicion_secundaria:
    # Bloque B - ejecuta SI condicion_primaria es False Y condicion_secundaria es True  
    resultado = "caso_secundario"
else:
    # Bloque C - ejecuta SI todas las condiciones anteriores son False
    resultado = "caso_por_defecto"
```

**Punto crucial**: Python evalúa en orden secuencial y **se detiene en el primer `True`**.

## 1.2 Casos de Uso Ideales para `if-elif-else`

### ✅ Escenarios perfectos:
- **Validaciones escalonadas** (de más restrictiva a menos restrictiva)
- **Clasificaciones por rangos** (edad, puntuaciones, niveles)
- **Decisiones con lógica heterogénea** (cada condición evalúa aspectos diferentes)

### Ejemplo del mundo real: Sistema de clasificación académica

```python
def clasificar_calificacion(puntaje):
    """Clasifica puntajes en sistema educativo"""
    if puntaje < 0 or puntaje > 100:
        return "INVÁLIDO"
    elif puntaje >= 90:
        return "SOBRESALIENTE (A)"
    elif puntaje >= 80:
        return "NOTABLE (B)" 
    elif puntaje >= 70:
        return "APROBATORIO (C)"
    elif puntaje >= 60:
        return "MÍNIMO (D)"
    else:
        return "REPROBADO (F)"
```

## 1.3 Patrones de Diseño con `if-elif-else`

### Patrón: Early Return (Retorno Temprano)
```python
# ❌ ANIDACIÓN PROFUNDA - EVITAR
def procesar_pedido(pedido):
    if pedido:
        if pedido.valido:
            if pedido.en_stock:
                if pedido.cliente_activo:
                    return "Éxito"
                else:
                    return "Cliente inactivo"
            else:
                return "Sin stock"
        else:
            return "Pedido inválido"
    else:
        return "Pedido nulo"

# ✅ EARLY RETURN - PREFERIBLE
def procesar_pedido(pedido):
    if not pedido:
        return "Pedido nulo"
    
    if not pedido.valido:
        return "Pedido inválido"
        
    if not pedido.en_stock:
        return "Sin stock"
        
    if not pedido.cliente_activo:
        return "Cliente inactivo"
        
    return "Éxito"
```

### Patrón: Condiciones Legibles
```python
# ❌ Condición críptica
if user and user.permissions and 'admin' in user.permissions and not user.suspended:

# ✅ Condición legible
usuario_es_admin_activo = (
    user and 
    user.permissions and 
    'admin' in user.permissions and 
    not user.suspended
)
if usuario_es_admin_activo:
```


# **2. Expresión Condicional (Operador Ternario)**

## 2.1 Sintaxis y Semántica

```python
# Estructura básica
valor = expresion_si_verdadero if condicion else expresion_si_falso

# Equivale a:
if condicion:
    valor = expresion_si_verdadero
else:
    valor = expresion_si_falso
```

## 2.2 Usos Apropiados vs Inapropiados

### ✅ Usos recomendados:
```python
# Asignaciones simples y claras
estado = "activo" if usuario.conectado else "inactivo"

# En estructuras de datos
configuracion = {
    "tema": "oscuro" if preferencias.modo_noche else "claro",
    "notificaciones": True
}

# En return statements
def obtener_categoria(edad):
    return "adulto" if edad >= 18 else "menor"
```

### ❌ Evitar en estos casos:
```python
# ❌ Ternario anidado - difícil de leer
nivel = "alto" if puntaje > 90 else "medio" if puntaje > 70 else "bajo"

# ❌ Lógica compleja
resultado = usuario.activar() if validar_permisos(usuario) and not usuario.bloqueado else registrar_error("Falló")
```


# **3. Alternativas a Switch-Case en Python**

## 3.1 Diccionarios de Funciones (Patrón de Dispatch)

### Caso de uso ideal: Sistemas de Comandos
```python
def accion_crear(datos):
    print(f"Creando recurso con: {datos}")
    return "creado"

def accion_actualizar(datos):
    print(f"Actualizando con: {datos}") 
    return "actualizado"

def accion_eliminar(datos):
    print(f"Eliminando recurso")
    return "eliminado"

# Diccionario de dispatch
comandos = {
    "crear": accion_crear,
    "actualizar": accion_actualizar, 
    "eliminar": accion_eliminar,
}

# Uso del patrón
def ejecutar_comando(nombre_comando, datos):
    funcion = comandos.get(nombre_comando)
    if funcion:
        return funcion(datos)
    else:
        return f"Comando '{nombre_comando}' no reconocido"

# Ejecución
resultado = ejecutar_comando("crear", {"nombre": "usuario123"})
```

### Ventajas clave:
- **Extensible**: Agregar comandos = agregar entradas al diccionario
- **Mantenible**: Cada función es independiente
- **Performance**: O(1) vs O(n) de múltiples elif

## 3.2 Diccionarios con Valores Simples

```python
# Para mapeos directos sin lógica compleja
colores_estado = {
    "exito": "verde",
    "error": "rojo", 
    "advertencia": "amarillo",
    "info": "azul"
}

color = colores_estado.get(estado, "gris")  # Valor por defecto
```

# **4. Match-Case (Python 3.10+) - Coincidencia de Patrones Estructurales**

## 4.1 Más que un Switch Tradicional

`match-case` no es solo para valores, sino para **patrones estructurales**.

## 4.2 Patrones Básicos

```python
# Coincidencia por valor
def manejar_estado(estado):
    match estado:
        case "activo":
            return "Sistema operativo"
        case "inactivo":
            return "Sistema en pausa"
        case "error":
            return "Sistema con fallas"
        case _:
            return "Estado desconocido"
```

## 4.3 Patrones Estructurales (Poder Real)

```python
# Coincidencia con tuplas
def procesar_comando(comando):
    match comando:
        case ("mover", x, y):
            return f"Moviendo a coordenadas ({x}, {y})"
        case ("atacar", objetivo):
            return f"Atacando a {objetivo}"
        case ("salir",):
            return "Saliendo del juego"
        case _:
            return "Comando no reconocido"

# Coincidencia con tipos
def identificar_tipo(dato):
    match dato:
        case int() | float():
            return "Número"
        case str():
            return "Texto"
        case list() | tuple():
            return "Secuencia"
        case _:
            return "Otro tipo"
```

## 4.4 Guardas (Condiciones Adicionales)

```python
def evaluar_puntaje(puntaje):
    match puntaje:
        case x if x >= 90:
            return "Excelente"
        case x if x >= 70:
            return "Bueno" 
        case x if x >= 50:
            return "Regular"
        case _:
            return "Insuficiente"
```

## 4.5 Patrones Avanzados con Clases

```python
class Punto:
    def __init__(self, x, y):
        self.x = x
        self.y = y

def analizar_punto(punto):
    match punto:
        case Punto(x=0, y=0):
            return "Origen"
        case Punto(x=0, y=y):
            return f"Sobre eje Y en {y}"
        case Punto(x=x, y=0):
            return f"Sobre eje X en {x}"
        case Punto(x=x, y=y):
            return f"En cuadrante: ({x}, {y})"
```

# **5. Técnicas Avanzadas y Optimizaciones**

## 5.1 Comparaciones Encadenadas (Chained Comparisons)

```python
# ✅ Legible y eficiente
if 18 <= edad < 65:
    print("Edad laboral")

# Equivale a:
if edad >= 18 and edad < 65:
    print("Edad laboral")
```

## 5.2 Short-Circuit Evaluation

```python
# Python evalúa solo hasta determinar el resultado
if usuario and usuario.tiene_permiso and usuario.activo:
    # Si 'usuario' es None, las demás condiciones NO se evalúan
    ejecutar_accion()

# Útil para evitar errores
if indice < len(lista) and lista[indice] == objetivo:
    # Evita IndexError
```

## 5.3 Condicionales en Comprensiones

```python
# Filtrado simple
pares = [x for x in range(100) if x % 2 == 0]

# Condicional en el valor
clasificaciones = [
    "par" if n % 2 == 0 else "impar" 
    for n in range(10)
]

# Múltiples condiciones
numeros = [
    n for n in range(20) 
    if n % 2 == 0 and n > 10 and n < 18
]
```

## 5.4 Patrón: Configuración Condicional

```python
config = {
    "ambiente": "produccion",
    "debug": False,
    "base_datos": {
        "url": os.getenv("DB_URL"),
        "timeout": 30 if os.getenv("AMBIENTE") == "prod" else 10,
    }
}
```


# **6. Guía de Elección: ¿Cuándo Usar Qué?**

## Decision Tree para Estructuras de Control

```
¿Necesitas evaluar condiciones?
├── ¿Son 1-3 condiciones simples? → if/elif/else
├── ¿Son mapeos valor→acción? 
│   ├── ¿Las acciones son funciones? → Diccionario de funciones  
│   └── ¿Las acciones son valores? → Diccionario simple
├── ¿Coincidencia de patrones complejos? → match-case
├── ¿Asignación condicional simple? → Ternario
└── ¿Filtrado de colecciones? → Comprensiones con if
```

## Tabla Comparativa Detallada

| Escenario | Herramienta Recomendada | Razón |
|-----------|------------------------|-------|
| Validación de formularios | `if-elif-else` con early return | Evaluación secuencial clara |
| Sistema de comandos | Diccionario de funciones | Extensibilidad y separación de concerns |
| Procesamiento de datos estructurados | `match-case` | Coincidencia de patrones poderosa |
| Configuración condicional | Operador ternario | Sintaxis compacta y legible |
| Routing de API | Diccionario de funciones | Facilita agregar nuevos endpoints |
| Análisis de tipos | `match-case` con patrones de tipo | Detección robusta de tipos |
| Transformación de listas | Comprensiones con if | Sintaxis funcional y eficiente |

---
