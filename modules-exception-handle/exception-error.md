# **Manejo de Errores y Excepciones en Python - Guía Completa Intermedio/Avanzado**

## 0. Visión General: Filosofía de Manejo de Errores en Python

Python adopta el principio "Es más fácil pedir perdón que permiso" (EAFP - Easier to Ask for Forgiveness than Permission):

- **EAFP vs LBYL**: Prefiere intentar operaciones y manejar errores vs verificar todo previamente
- **Sistema de excepciones robusto**: Jerarquía rica y extensible de excepciones
- **Múltiples patrones**: Desde manejo básico hasta técnicas avanzadas con context managers y decoradores
- **Cultura Pythonica**: Errores explícitos y informativos sobre fallos silenciosos

---

# **1. Jerarquía de Excepciones - Conocimiento Fundamental**

## 1.1 Árbol Completo de Excepciones Built-in

```python
BaseException
 ├── SystemExit
 ├── KeyboardInterrupt
 ├── GeneratorExit
 └── Exception
      ├── ArithmeticError
      │    ├── ZeroDivisionError
      │    ├── FloatingPointError
      │    ├── OverflowError
      │    └── ...
      ├── LookupError
      │    ├── IndexError
      │    └── KeyError
      ├── OSError
      │    ├── FileNotFoundError
      │    ├── PermissionError
      │    ├── TimeoutError
      │    └── ...
      ├── ValueError
      ├── TypeError
      ├── RuntimeError
      │    └── NotImplementedError
      ├── AttributeError
      ├── NameError
      └── ...
```

## 1.2 Errores Comunes y Cuándo Ocurren

```python
# ValueError - valor inapropiado pero tipo correcto
int("no_es_numero")           # ❌
float("abc")                  # ❌

# TypeError - operación en tipo incorrecto
"string" + 42                 # ❌
len(123)                      # ❌

# KeyError - clave no existe en diccionario
d = {"a": 1}
d["b"]                        # ❌

# IndexError - índice fuera de rango
lista = [1, 2, 3]
lista[10]                     # ❌

# AttributeError - atributo no existe
"texto".append("x")           # ❌

# ZeroDivisionError - división por cero
1 / 0                         # ❌

# FileNotFoundError - archivo no existe
open("archivo_inexistente.txt")  # ❌
```

## 1.3 Conociendo las Excepciones por Categoría

```python
def demostrar_errores_comunes():
    """Ejemplos didácticos de errores comunes"""
    
    # Lookup Errors
    try:
        diccionario = {"a": 1}
        print(diccionario["inexistente"])  # KeyError
    except KeyError as e:
        print(f"KeyError: {e}")
    
    try:
        lista = [1, 2, 3]
        print(lista[10])  # IndexError
    except IndexError as e:
        print(f"IndexError: {e}")
    
    # Type/Value Errors
    try:
        resultado = "texto" + 42  # TypeError
    except TypeError as e:
        print(f"TypeError: {e}")
    
    try:
        numero = int("no_numerico")  # ValueError
    except ValueError as e:
        print(f"ValueError: {e}")
    
    # OS Errors
    try:
        with open("archivo_que_no_existe.txt") as f:
            pass  # FileNotFoundError
    except FileNotFoundError as e:
        print(f"FileNotFoundError: {e}")

# demostrar_errores_comunes()
```

---

# **2. Manejo Básico de Excepciones - Más Allá de Try/Except**

## 2.1 Estructura Completa de Try/Except

```python
try:
    # Código que puede generar excepciones
    resultado = operacion_riesgosa()
    
except TipoExcepcionEspecifica as e:
    # Manejo de excepción específica
    print(f"Error específico: {e}")
    
except (OtraExcepcion, YOtraMas) as e:
    # Manejo múltiple de excepciones relacionadas
    print(f"Error múltiple: {e}")
    
except Exception as e:
    # Captura cualquier excepción (usar con cuidado)
    print(f"Error inesperado: {e}")
    
else:
    # Se ejecuta solo si NO hubo excepciones
    print(f"Éxito: {resultado}")
    
finally:
    # Siempre se ejecuta, haya o no excepciones
    print("Limpieza completada")
```

## 2.2 Ejemplos Prácticos y Reales

```python
def procesar_entrada_usuario(entrada):
    """Procesa entrada de usuario con manejo robusto de errores"""
    try:
        numero = float(entrada)
        
    except ValueError as e:
        print(f"Error: '{entrada}' no es un número válido")
        return None
        
    except (TypeError, AttributeError) as e:
        print(f"Error de tipo: {e}")
        return None
        
    else:
        # Solo si la conversión fue exitosa
        if numero < 0:
            print("Advertencia: Número negativo")
        return numero
        
    finally:
        print("Procesamiento de entrada completado")

# Pruebas
print(procesar_entrada_usuario("3.14"))    # Éxito
print(procesar_entrada_usuario("abc"))     # ValueError
print(procesar_entrada_usuario(""))        # ValueError
```

## 2.3 Uso de `else` para Código de Éxito

```python
def cargar_configuracion(ruta_archivo):
    """Carga configuración con manejo claro de éxito/error"""
    try:
        with open(ruta_archivo, 'r') as archivo:
            contenido = archivo.read()
            
    except FileNotFoundError:
        print(f"Archivo de configuración no encontrado: {ruta_archivo}")
        return crear_configuracion_por_defecto()
        
    except PermissionError:
        print(f"Sin permisos para leer: {ruta_archivo}")
        return None
        
    else:
        # Solo se ejecuta si el archivo se leyó correctamente
        print("Configuración cargada exitosamente")
        return parsear_configuracion(contenido)
        
    finally:
        print("Operación de carga de configuración finalizada")

def crear_configuracion_por_defecto():
    print("Creando configuración por defecto")
    return {"modo": "default", "timeout": 30}

def parsear_configuracion(contenido):
    # Simulación de parsing
    return {"modo": "desde_archivo", "contenido": contenido}
```

## 2.4 `finally` para Gestión de Recursos

```python
def procesar_archivo_temporal(ruta):
    """Garantiza que el archivo temporal se elimine siempre"""
    archivo = None
    try:
        archivo = open(ruta, 'w')
        archivo.write("datos temporales")
        # Simular un error
        if "error" in ruta:
            raise RuntimeError("Error simulado durante procesamiento")
        return "Procesamiento exitoso"
        
    except IOError as e:
        print(f"Error de E/S: {e}")
        return None
        
    finally:
        # Esto se ejecuta SIEMPRE, haya error o no
        if archivo:
            archivo.close()
            print("Archivo cerrado")
        # En un caso real, aquí eliminaríamos el archivo temporal
        print("Limpieza de recursos completada")

# Prueba
resultado = procesar_archivo_temporal("/tmp/datos.txt")
print(f"Resultado: {resultado}")
```

---

# **3. Lanzamiento de Excepciones - Control Activo de Errores**

## 3.1 `raise` para Lanzar Excepciones

```python
def transferir_fondos(cuenta_origen, cuenta_destino, monto):
    """Función de transferencia con validaciones explícitas"""
    
    # Validaciones tempranas (fail fast)
    if monto <= 0:
        raise ValueError("El monto debe ser positivo")
    
    if not cuenta_origen.activa:
        raise ValueError("Cuenta de origen inactiva")
    
    if not cuenta_destino.activa:
        raise ValueError("Cuenta de destino inactiva")
    
    if cuenta_origen.saldo < monto:
        raise ValueError("Fondos insuficientes")
    
    # Lógica de transferencia
    try:
        cuenta_origen.saldo -= monto
        cuenta_destino.saldo += monto
        return True
        
    except Exception as e:
        # Revertir en caso de error
        cuenta_origen.saldo += monto
        cuenta_destino.saldo -= monto
        raise RuntimeError(f"Error en transferencia: {e}") from e

# Clases de ejemplo
class Cuenta:
    def __init__(self, saldo_inicial=0, activa=True):
        self.saldo = saldo_inicial
        self.activa = activa

# Pruebas
cuenta1 = Cuenta(1000)
cuenta2 = Cuenta(500)

try:
    transferir_fondos(cuenta1, cuenta2, 1500)  # ❌ Fondos insuficientes
except ValueError as e:
    print(f"Error de validación: {e}")

try:
    transferir_fondos(cuenta1, cuenta2, -100)  # ❌ Monto negativo
except ValueError as e:
    print(f"Error de validación: {e}")
```

## 3.2 Re-lanzamiento de Excepciones con Contexto

```python
def procesar_datos_complejos(datos):
    """Procesa datos y añade contexto a los errores"""
    try:
        resultado = transformar_datos(datos)
        return validar_resultado(resultado)
        
    except ValueError as e:
        # Añadir contexto y re-lanzar
        raise ValueError(f"Error procesando datos '{datos}': {e}") from e
        
    except TypeError as e:
        # Log y re-lanzar
        print(f"Error de tipo detectado: {e}")
        raise

def transformar_datos(datos):
    if not datos:
        raise ValueError("Datos vacíos")
    return datos.upper()

def validar_resultado(resultado):
    if len(resultado) > 100:
        raise ValueError("Resultado demasiado largo")
    return resultado

# Prueba
try:
    procesar_datos_complejos("")  # Datos vacíos
except ValueError as e:
    print(f"Error capturado: {e}")
    print(f"Causa original: {e.__cause__}")
```

## 3.3 Encadenamiento de Excepciones con `from`

```python
def cargar_y_procesar_configuracion(ruta):
    """Demonstra encadenamiento de excepciones"""
    try:
        with open(ruta, 'r') as f:
            config = json.load(f)
            
    except FileNotFoundError as e:
        raise ConfiguracionError(f"Archivo no encontrado: {ruta}") from e
        
    except json.JSONDecodeError as e:
        raise ConfiguracionError(f"JSON inválido en {ruta}") from e
        
    try:
        return validar_configuracion(config)
        
    except ValueError as e:
        raise ConfiguracionError(f"Configuración inválida") from e

class ConfiguracionError(Exception):
    """Excepción personalizada para errores de configuración"""
    pass

def validar_configuracion(config):
    if not isinstance(config, dict):
        raise ValueError("Configuración debe ser un diccionario")
    return config
```

---

# **4. Excepciones Personalizadas - Diseño Profesional**

## 4.1 Creación de Excepciones con Información Rica

```python
class ErrorAplicacion(Exception):
    """Clase base para todas las excepciones de la aplicación"""
    
    def __init__(self, mensaje, codigo_error=None, detalles=None):
        self.mensaje = mensaje
        self.codigo_error = codigo_error
        self.detalles = detalles or {}
        super().__init__(self.mensaje)
    
    def __str__(self):
        if self.codigo_error:
            return f"[{self.codigo_error}] {self.mensaje}"
        return self.mensaje
    
    def to_dict(self):
        """Convierte la excepción a diccionario para APIs"""
        return {
            "error": self.mensaje,
            "codigo": self.codigo_error,
            "detalles": self.detalles
        }

class UsuarioInvalidoError(ErrorAplicacion):
    """Excepción para errores relacionados con usuarios"""
    
    def __init__(self, usuario_id, razon, detalles=None):
        mensaje = f"Usuario inválido: {usuario_id} - {razon}"
        super().__init__(
            mensaje=mensaje,
            codigo_error="USUARIO_INVALIDO",
            detalles=detalles or {"usuario_id": usuario_id, "razon": razon}
        )

class SaldoInsuficienteError(ErrorAplicacion):
    """Excepción para fondos insuficientes"""
    
    def __init__(self, usuario_id, saldo_actual, monto_solicitado):
        mensaje = f"Saldo insuficiente para {usuario_id}"
        detalles = {
            "usuario_id": usuario_id,
            "saldo_actual": saldo_actual,
            "monto_solicitado": monto_solicitado,
            "diferencia": monto_solicitado - saldo_actual
        }
        super().__init__(
            mensaje=mensaje,
            codigo_error="SALDO_INSUFICIENTE",
            detalles=detalles
        )
```

## 4.2 Uso de Excepciones Personalizadas

```python
class SistemaBancario:
    def __init__(self):
        self.usuarios = {}
        self.saldos = {}
    
    def registrar_usuario(self, usuario_id, nombre):
        """Registra un nuevo usuario"""
        if not usuario_id or not isinstance(usuario_id, str):
            raise UsuarioInvalidoError(
                usuario_id, 
                "ID debe ser string no vacío"
            )
        
        if usuario_id in self.usuarios:
            raise UsuarioInvalidoError(
                usuario_id, 
                "Usuario ya existe"
            )
        
        self.usuarios[usuario_id] = nombre
        self.saldos[usuario_id] = 0
        print(f"Usuario {usuario_id} registrado exitosamente")
    
    def retirar_fondos(self, usuario_id, monto):
        """Retira fondos de una cuenta"""
        if usuario_id not in self.usuarios:
            raise UsuarioInvalidoError(usuario_id, "Usuario no existe")
        
        if monto <= 0:
            raise ValueError("El monto debe ser positivo")
        
        saldo_actual = self.saldos[usuario_id]
        if saldo_actual < monto:
            raise SaldoInsuficienteError(usuario_id, saldo_actual, monto)
        
        self.saldos[usuario_id] -= monto
        return self.saldos[usuario_id]

# Prueba del sistema
sistema = SistemaBancario()

try:
    sistema.registrar_usuario("", "Nombre")  # ❌ ID vacío
except UsuarioInvalidoError as e:
    print(f"Error: {e}")
    print(f"Detalles: {e.detalles}")

try:
    sistema.registrar_usuario("user123", "Ana")
    sistema.retirar_fondos("user123", 100)  # ❌ Saldo insuficiente
except SaldoInsuficienteError as e:
    print(f"Error: {e}")
    print(f"Detalles: {e.detalles}")
    print(f"Representación JSON: {e.to_dict()}")
```

---

# **5. Patrones Avanzados de Manejo de Errores**

## 5.1 EAFP vs LBYL (Look Before You Leap)

```python
# LBYL - Verificar antes de actuar (menos Pythonico)
def procesar_datos_lbyl(datos):
    if datos is None:
        return None
    if not isinstance(datos, list):
        return None
    if len(datos) == 0:
        return None
    if not all(isinstance(x, (int, float)) for x in datos):
        return None
    
    return sum(datos) / len(datos)

# EAFP - Intentar y manejar errores (más Pythonico)
def procesar_datos_eafp(datos):
    try:
        return sum(datos) / len(datos)
    except (TypeError, ZeroDivisionError, AttributeError):
        return None

# Mejor enfoque: combinación con excepciones específicas
def procesar_datos_hibrido(datos):
    try:
        if not datos:
            raise ValueError("Datos vacíos")
        return sum(datos) / len(datos)
    
    except TypeError as e:
        print(f"Error de tipo: {e}")
        return None
    
    except ZeroDivisionError:
        print("No se puede promediar lista vacía")
        return None
    
    except Exception as e:
        print(f"Error inesperado: {e}")
        return None
```

## 5.2 Manejo de Errores como Control de Flujo

```python
class Cache:
    def __init__(self):
        self._cache = {}
    
    def obtener(self, clave, valor_por_defecto=None):
        """Obtiene valor del cache o retorna valor por defecto"""
        try:
            return self._cache[clave]
        except KeyError:
            return valor_por_defecto
    
    def obtener_o_calcular(self, clave, funcion_calculo):
        """Obtiene del cache o calcula y almacena"""
        try:
            return self._cache[clave]
        except KeyError:
            valor = funcion_calculo()
            self._cache[clave] = valor
            return valor

# Uso del patrón
cache = Cache()

# Patrón común: intentar obtener, si falla calcular
resultado = cache.obtener("clave_costosa")
if resultado is None:
    resultado = calcular_valor_costoso()
    cache._cache["clave_costosa"] = resultado
```

## 5.3 Decoradores para Manejo Automático de Errores

```python
from functools import wraps
import time
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

def manejar_errores(reintentos=0, delay=1, excepciones_ignoradas=None):
    """
    Decorador para manejo automático de errores con reintentos
    """
    excepciones_ignoradas = excepciones_ignoradas or (ValueError,)
    
    def decorador(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            ultimo_error = None
            
            for intento in range(reintentos + 1):
                try:
                    return func(*args, **kwargs)
                    
                except excepciones_ignoradas as e:
                    # Re-lanzar excepciones que no deben reintentarse
                    raise
                    
                except Exception as e:
                    ultimo_error = e
                    if intento < reintentos:
                        logger.warning(
                            f"Intento {intento + 1}/{reintentos + 1} falló. "
                            f"Reintentando en {delay} segundos. Error: {e}"
                        )
                        time.sleep(delay)
                    else:
                        logger.error(
                            f"Todos los intentos fallaron para {func.__name__}. "
                            f"Último error: {e}"
                        )
            
            raise ultimo_error
        return wrapper
    return decorador

def solo_connection_errors(reintentos=3):
    """Reintenta solo para errores de conexión"""
    return manejar_errores(
        reintentos=reintentos,
        excepciones_ignoradas=(ValueError, TypeError, KeyError)
    )

# Uso de los decoradores
@solo_connection_errors(reintentos=2)
def descargar_datos(url):
    """Simula descarga que puede fallar por conexión"""
    import random
    if random.random() < 0.7:  # 70% de probabilidad de fallo
        raise ConnectionError("Error de conexión simulado")
    return f"Datos de {url}"

@manejar_errores(reintentos=1)
def procesar_archivo(ruta):
    """Procesa archivo con manejo automático de errores"""
    with open(ruta, 'r') as f:
        return f.read()

# Pruebas
try:
    resultado = descargar_datos("https://ejemplo.com")
    print(f"Éxito: {resultado}")
except Exception as e:
    print(f"Fallo final: {e}")
```

---

# **6. Context Managers Avanzados - Gestión Robusta de Recursos**

## 6.1 Implementación con Clase

```python
class ManejadorTransaccion:
    """Context manager para manejo de transacciones con rollback automático"""
    
    def __init__(self, recurso, modo="escritura"):
        self.recurso = recurso
        self.modo = modo
        self.estado_original = None
        self.transaccion_activa = False
    
    def __enter__(self):
        print(f"Iniciando transacción en modo {self.modo}")
        
        # Guardar estado original para posible rollback
        self.estado_original = self._capturar_estado()
        
        # Iniciar transacción
        self.transaccion_activa = True
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Finalizando transacción")
        
        if exc_type is not None:
            # Hubo una excepción - hacer rollback
            print("Error detectado, realizando rollback")
            self._restaurar_estado(self.estado_original)
        else:
            # Todo bien - commit implícito
            print("Transacción completada exitosamente")
        
        self.transaccion_activa = False
        # No suprimir excepciones
        return False
    
    def _capturar_estado(self):
        """Captura el estado actual para posible rollback"""
        return f"Estado de {self.recurso} en {id(self)}"
    
    def _restaurar_estado(self, estado):
        """Restaura el estado desde la copia de seguridad"""
        print(f"Restaurando estado: {estado}")

# Uso
try:
    with ManejadorTransaccion("base_de_datos") as transaccion:
        print("Realizando operaciones...")
        # Simular error
        raise ValueError("Error durante la transacción")
except ValueError as e:
    print(f"Error manejado: {e}")
```

## 6.2 Implementación con `contextlib`

```python
from contextlib import contextmanager
import time

@contextmanager
def temporizador_operacion(nombre_operacion):
    """Context manager para medir tiempo de operaciones"""
    inicio = time.time()
    try:
        print(f"🚀 Iniciando: {nombre_operacion}")
        yield
        
    except Exception as e:
        fin = time.time()
        duracion = fin - inicio
        print(f"❌ {nombre_operacion} falló después de {duracion:.2f}s: {e}")
        raise
        
    else:
        fin = time.time()
        duracion = fin - inicio
        print(f"✅ {nombre_operacion} completado en {duracion:.2f}s")

@contextmanager
def manejar_recurso_con_reintentos(recurso, reintentos=3):
    """Maneja recursos con reintentos automáticos"""
    ultimo_error = None
    
    for intento in range(reintentos):
        try:
            print(f"Intento {intento + 1} de acceder a {recurso}")
            # Simular obtención del recurso
            yield f"{recurso}_manejado"
            # Si llegamos aquí, éxito
            return
            
        except Exception as e:
            ultimo_error = e
            if intento < reintentos - 1:
                print(f"Reintentando en 1 segundo...")
                time.sleep(1)
    
    # Si todos los intentos fallaron
    raise ultimo_error

# Uso combinado
try:
    with temporizador_operacion("Descarga con reintentos"):
        with manejar_recurso_con_reintentos("servicio_externo", reintentos=2) as recurso:
            print(f"Usando recurso: {recurso}")
            # Simular fallo ocasional
            if hasattr(recurso, "simular_fallo"):
                raise ConnectionError("Error de conexión simulado")
                
except Exception as e:
    print(f"Operación falló definitivamente: {e}")
```

---

# **7. Manejo de Errores en Casos Específicos**

## 7.1 Archivos y E/S

```python
def leer_configuracion_robusta(ruta_archivo, config_por_defecto=None):
    """Lee configuración con múltiples fallbacks"""
    config_por_defecto = config_por_defecto or {}
    
    # Intentar diferentes ubicaciones
    rutas_posibles = [
        ruta_archivo,
        f"./{ruta_archivo}",
        f"../{ruta_archivo}",
        "/etc/mi_app/config.json"
    ]
    
    for ruta in rutas_posibles:
        try:
            with open(ruta, 'r', encoding='utf-8') as archivo:
                contenido = archivo.read()
                print(f"Configuración cargada desde: {ruta}")
                return json.loads(contenido)
                
        except FileNotFoundError:
            continue  # Intentar siguiente ruta
            
        except PermissionError as e:
            print(f"Sin permisos para leer {ruta}: {e}")
            continue
            
        except json.JSONDecodeError as e:
            print(f"JSON inválido en {ruta}: {e}")
            # Podríamos continuar o fallar según la política
    
    # Si ninguna ruta funcionó
    print("Usando configuración por defecto")
    return config_por_defecto

# Prueba
config = leer_configuracion_robusta("config.json")
print(f"Configuración final: {config}")
```

## 7.2 Operaciones de Red

```python
import requests
from requests.exceptions import RequestException

def descargar_con_resiliencia(url, timeout=10, reintentos=3):
    """Descarga con manejo robusto de errores de red"""
    
    for intento in range(reintentos):
        try:
            respuesta = requests.get(url, timeout=timeout)
            respuesta.raise_for_status()  # Lanza excepción para códigos 4xx/5xx
            return respuesta.content
            
        except requests.exceptions.Timeout:
            print(f"Timeout en intento {intento + 1}")
            if intento == reintentos - 1:
                raise
            
        except requests.exceptions.ConnectionError as e:
            print(f"Error de conexión en intento {intento + 1}: {e}")
            if intento == reintentos - 1:
                raise
            
        except requests.exceptions.HTTPError as e:
            print(f"Error HTTP {e.response.status_code} en intento {intento + 1}")
            # No reintentar para errores del cliente (4xx)
            if 400 <= e.response.status_code < 500:
                raise
            if intento == reintentos - 1:
                raise
        
        except RequestException as e:
            print(f"Error de request en intento {intento + 1}: {e}")
            if intento == reintentos - 1:
                raise
        
        # Esperar antes del reintento
        if intento < reintentos - 1:
            time.sleep(2 ** intento)  # Backoff exponencial
    
    raise RequestException("Todos los reintentos fallaron")

# Uso
try:
    contenido = descargar_con_resiliencia(
        "https://httpbin.org/delay/5",  # Simula timeout
        timeout=3,
        reintentos=2
    )
    print("Descarga exitosa")
except RequestException as e:
    print(f"Descarga fallida: {e}")
```

---

# **8. Buenas Prácticas y Anti-patrones**

## 8.1 Qué Hacer y Qué No Hacer

```python
# ❌ ANTI-PATRONES - EVITAR

# 1. Captura demasiado amplia y silenciosa
try:
    operacion_peligrosa()
except:  # ❌ Captura hasta SystemExit y KeyboardInterrupt
    pass

# 2. Ignorar excepciones específicas
try:
    procesar_datos()
except ValueError:
    pass  # ❌ Error silenciado

# 3. Usar excepciones para control de flujo normal
def buscar_elemento(lista, objetivo):
    try:
        return lista.index(objetivo)
    except ValueError:
        return -1  # ❌ Mejor usar 'if objetivo in lista'

# 4. Excepciones genéricas sin información
raise Exception("Algo salió mal")  # ❌ Poco informativo

# ✅ BUENAS PRÁCTICAS - SEGUIR

# 1. Captura específica
try:
    resultado = int(entrada_usuario)
except ValueError as e:
    logger.error(f"Entrada inválida: {entrada_usuario} - {e}")
    raise

# 2. Fail fast con validaciones tempranas
def transferir_fondos(monto, cuenta):
    if monto <= 0:
        raise ValueError("Monto debe ser positivo")  # ✅
    if not cuenta.activa:
        raise ValueError("Cuenta inactiva")  # ✅
    # ... lógica principal

# 3. Excepciones informativas
class SaldoInsuficienteError(Exception):
    def __init__(self, saldo_actual, monto_solicitado):
        super().__init__(
            f"Saldo insuficiente: {saldo_actual}, se solicitó: {monto_solicitado}"
        )

# 4. Uso apropiado de finally para limpieza
recurso = adquirir_recurso()
try:
    usar_recurso(recurso)
finally:
    liberar_recurso(recurso)  # ✅ Siempre se ejecuta
```

## 8.2 Principio de Responsabilidad Única para Errores

```python
class ProcesadorDatos:
    def __init__(self):
        self.validadores = []
        self.transformadores = []
    
    def agregar_validador(self, validador):
        self.validadores.append(validador)
    
    def agregar_transformador(self, transformador):
        self.transformadores.append(transformador)
    
    def procesar(self, datos):
        # Fase 1: Validación
        for validador in self.validadores:
            try:
                validador(datos)
            except ValueError as e:
                raise DatosInvalidosError(f"Validación falló: {e}") from e
        
        # Fase 2: Transformación
        resultado = datos
        for transformador in self.transformadores:
            try:
                resultado = transformador(resultado)
            except Exception as e:
                raise TransformacionError(f"Transformación falló: {e}") from e
        
        return resultado

class DatosInvalidosError(Exception):
    pass

class TransformacionError(Exception):
    pass

# Uso
procesador = ProcesadorDatos()
procesador.agregar_validador(lambda d: None if not d else ...)
procesador.agregar_transformador(lambda d: d.upper())

try:
    resultado = procesador.procesar("datos de prueba")
    print(f"Resultado: {resultado}")
except DatosInvalidosError as e:
    print(f"Error de datos: {e}")
except TransformacionError as e:
    print(f"Error de transformación: {e}")
```

---

# **9. Guía de Elección: ¿Cuándo Usar Qué Técnica?**

## Árbol de Decisión para Manejo de Errores

```
¿Necesitas manejar un error?
├── ¿Es un error esperado del dominio? → Excepción personalizada
├── ¿Es un error técnico/inesperado? → Excepción built-in
└── ¿Es parte del flujo normal? → Valor de retorno especial

¿Cómo manejar el error?
├── ¿Puede recuperarse? → try/except con lógica de recuperación
├── ¿Requiere limpieza? → finally o context manager
└── ¿Es transitorio? → Decorador con reintentos

¿Dónde manejar el error?
├── ¿En el lugar donde ocurre? → Manejo local
├── ¿En un nivel superior? → Propagar con raise
└── ¿En múltiples lugares? → Decorador o middleware
```

## Tabla de Técnicas por Escenario

| Escenario | Técnica Recomendada | Ejemplo |
|-----------|---------------------|---------|
| Validación de entrada | Fail fast con excepciones | `if not valido: raise ValueError()` |
| Acceso a recursos | Context managers | `with open(...) as f:` |
| Operaciones de red | Reintentos con backoff | Decorador con `@reintentar(n=3)` |
| Procesamiento por lotes | Continuar tras errores | `try/except` dentro del bucle |
| APIs públicas | Excepciones personalizadas ricas | `class MiError(Exception):` |
| Limpieza de recursos | `finally` o `__exit__` | Cerrar conexiones en `finally` |

---
