# 🟦 **Nivel Intermedio — 20 Ejercicios Combinados**

## 1. Contador de Letras
Escribe una función que reciba un texto y devuelva cuántas veces aparece cada letra, usando `Counter`. Maneja el error si la entrada no es `str`.

## 2. Validador de Números
Crea una función que valide si una cadena es un número entero o flotante usando excepciones y métodos de string.

## 3. Procesador de Texto
Dado un texto, usa slicing y métodos avanzados de strings para invertir palabras, eliminar espacios extra y devolverlo limpio en lowercase.

## 4. Filtro de Números Válidos
Implementa una función que reciba una lista de valores mixtos y devuelva solo los que sean números válidos, usando try/except.

## 5. Calculadora con Excepciones Personalizadas
Escribe un módulo con funciones para sumar, restar, multiplicar y dividir, lanzando tus propias excepciones para división entre cero.

## 6. Ordenamiento Avanzado
Dada una lista de diccionarios con `nombre` y `edad`, ordena usando `sorted` con `lambda` por edad, luego por longitud del nombre.

## 7. Contador de Archivos
Escribe una función que reciba una ruta y devuelva cuántos archivos tiene dentro, usando `pathlib`.

## 8. Parser de Configuración
Escribe un parser simple que reciba un string tipo `"clave=valor"` y lo convierta en dict usando `partition()`.

## 9. Generador de Contraseñas
Crea una función que genere una contraseña aleatoria con letras, números y símbolos, usando `random` y `string`.

## 10. Contador de Palabras en Archivo
Toma un archivo `.txt`, lee línea por línea y devuelve la cantidad total de palabras, usando manejo de archivos + manejo de excepciones.

## 11. Eliminador de Duplicados
Crea una función que reciba una lista y devuelva otra lista sin duplicados pero conservando orden, usando un `set` auxiliar.

## 12. Validador de Email
Implementa una validación de email usando expresiones regulares; si no coincide, lanza `ValueError`.

## 13. Conversor de Fechas
Crea una función que convierta una fecha string `"YYYY-MM-DD"` a un objeto `datetime` y luego la formatee a tres formatos distintos.

## 14. Verificador de Capicúa
Dado un número entero, usa slicing sobre strings para verificar si es capicúa.

## 15. Serializador JSON
Crea funciones que serialicen y deserialicen diccionarios a JSON. Maneja errores de archivo y formato con try/except.

## 16. Decorador de Tiempo
Escribe un decorador que imprima el tiempo que tarda cualquier función en ejecutarse.

## 17. Agrupador de Palabras
Implementa una función que reciba palabras y devuelva un dict agrupándolas por su primera letra usando `defaultdict(list)`.

## 18. Transformador de Vocales
Escribe una función que reciba un string y reemplace vocales por números usando `str.maketrans()` y `translate()`.

## 19. Intercalador de Listas
Crea una función que reciba dos listas y devuelva una lista con sus elementos intercalados usando `itertools.zip_longest`.

## 20. Contador con Closure
Implementa un contador con closures: cada vez que llames a la función, incrementa un valor interno.

---

# 🟩 **Nivel Avanzado — 20 Ejercicios Combinados**

## 21. Tokenizador de Texto
Construye un sistema que convierta texto en tokens (palabras, números, símbolos), usando `re` y manejo robusto de errores.

## 22. Validador de Archivos
Escribe un validador de archivos que verifique extensión, tamaño y fecha de modificación usando `pathlib` y `datetime`.

## 23. Cache con Decorador
Implementa un decorador que cachee resultados usando un diccionario interno (simulando un mini `lru_cache`).

## 24. Procesador de CSV Grande
Crea una función que analice un CSV grande sin cargarlo completo en memoria, procesando línea por línea.

## 25. Evaluador Seguro de Expresiones
Construye una función que reciba una expresión aritmética (como string) y la evalúe de forma segura (sin usar `eval`).

## 26. Serializador de Objetos
Escribe un serializador que convierta un objeto con atributos en JSON usando `__dict__`, y otro que reconstruya el objeto.

## 27. Motor de Búsqueda por Prefijo
Implementa un mini-motor de búsqueda que, dado un conjunto de textos, devuelva coincidencias por prefijo usando slicing y filtros.

## 28. Módulo de Métricas Matemáticas
Crea un módulo con funciones matemáticas que usen `math` y `statistics` para calcular métricas a partir de una lista.

## 29. Manipulador de Binarios
Escribe funciones que manipulen binarios usando `bytearray`: insertar, eliminar y reemplazar rangos específicos.

## 30. Decorador de Validación de Rangos
Implementa un decorador que valide rangos de entradas: si el valor no está entre min y max, lanza excepción.

## 31. Generador de Números Primos
Construye un generador (generator) que produzca números primos infinitamente, usando `yield`.

## 32. Sistema de Logs con Decorador
Implementa un motor de logs que imprima hora exacta, función llamada y argumentos usando un decorador.

## 33. Normalizador de Texto
Escribe un procesador que reciba un texto, lo normalice con `unicodedata`, elimine tildes y símbolos, y devuelva solo letras.

## 34. Combinador de JSONs
Implementa un sistema que lea JSONs desde un directorio y combine todos en una sola estructura, con manejo de errores robusto.

## 35. Contador de Claves Anidadas
Crea una función que recorra una estructura compleja (listas + diccionarios anidados) recursivamente para contar claves totales.

## 36. Convertidor de Casos
Implementa un mini parser que convierta texto en snake_case, camelCase y PascalCase usando métodos de string.

## 37. Motor de Plantillas
Construye un motor de plantillas: dado un texto con `{var}`, reemplázalo con valores de un dict usando `format_map`.

## 38. Calculador de Tamaño de Directorio
Implementa un sistema para leer rutas y calcular tamaño total de archivos usando `pathlib.rglob()`.

## 39. Decorador de Reintentos
Escribe un decorador que reintente ejecutar una función N veces si lanza excepciones, con espera entre intentos.

## 40. Generador de Productos Cartesianos
Crea una función que reciba listas de diferentes tamaños y calcule sus productos cartesianos usando `itertools.product`.

---

# 🟥 **Nivel Competitivo — 20 Ejercicios**

## 41. Subcadena Sin Repetidos
Dado un string muy largo (hasta 10⁶ caracteres), encuentra la subcadena más larga sin caracteres repetidos usando sliding window.

## 42. Palabra Más Frecuente
Dado un texto, encuentra la palabra que más aparece usando `Counter`. Si hay empate, devuelve la lexicográficamente menor.

## 43. Subarreglo de Suma Máxima
Dado un arreglo, encuentra el subarreglo contiguo con mayor suma (Kadane), implementado con manejo de errores y sin librerías externas.

## 44. Verificador de Anagramas
Verifica si dos cadenas son anagramas usando un diccionario de frecuencias optimizado y evitando sorting.

## 45. Detector de Ciclos
Implementa un algoritmo que detecte ciclos en una lista enlazada representada como diccionario (`next` pointers simulados).

## 46. Operaciones con Números Grandes
Dado un número entero grande representado como string, implementa suma y resta manual sin convertir a int.

## 47. Búsqueda Binaria
Implementa búsqueda binaria recursiva y iterativa sobre listas ya ordenadas, con validación de tipos.

## 48. Primer Duplicado Cercano
Dado un arreglo de enteros, encuentra el primer duplicado con la menor distancia entre apariciones usando un set.

## 49. Siguiente String Lexicográfico
Dado un texto, genera el siguiente string lexicográfico (como en problemas de programación competitiva).

## 50. Número Faltante con XOR
Encuentra el número faltante en una lista del 1 al N usando XOR.

## 51. Reordenamiento Pares-Impares
Dado un arreglo de ints, reordénalo para que pares vayan antes que impares, usando orden estable y sin crear nueva lista.

## 52. Split Personalizado
Implementa tu propia función `split` (sin usar `.split`) para dividir texto por un delimitador.

## 53. Rotación de Arreglo
Dado un arreglo, rota sus elementos K posiciones a la derecha usando slicing y operaciones O(1) adicionales.

## 54. Diccionario Hash Personalizado
Implementa tu propio diccionario hash con resolución por chaining (listas), usando módulos y funciones separadas.

## 55. Validador de Paréntesis Balanceados
Verifica si una expresión con paréntesis, llaves y corchetes está balanceada usando una pila implementada con lista.

## 56. Algoritmo de Dos Punteros
Implementa el algoritmo de dos punteros para encontrar pares que suman un objetivo, en un arreglo ordenado.

## 57. Conversor a Binario
Convierte un número entero a binario sin usar `bin()`, mediante operaciones bitwise.

## 58. Prefijo Común Más Largo
Encuentra el "longest prefix match": dado un conjunto de palabras, devuelve el prefijo más largo que comparten todas.

## 59. Generador de Permutaciones
Genera todas las permutaciones de un string usando recursión y sets para evitar duplicados.

## 60. Implementación de Trie
Implementa un trie (árbol de prefijos) básico para insertar y buscar palabras.