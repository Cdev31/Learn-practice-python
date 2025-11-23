# 🟦 **Nivel Intermedio — Set 3 (20 ejercicios nuevos)**

## 1. Ordenador de Palabras por Longitud
Crea una función que reciba una frase y devuelva las palabras ordenadas por longitud, ignorando signos y tildes.

## 2. Analizador de Strings
Escribe una función que transforme una lista de strings a una lista donde cada elemento es `(string_original, largo, cantidad_vocales)`.

## 3. Validador ASCII
Implementa un validador que reciba una cadena y determine si contiene únicamente ASCII usando `ord()`.

## 4. Conversor de Fechas Robusto
Convierte una lista de fechas en formato `"DD-MM-YYYY"` a objetos `datetime`, ignorando entradas inválidas.

## 5. Filtro de Líneas Numéricas
Crea una función que reciba un archivo y devuelva solo las líneas que contienen números enteros.

## 6. Inversor de Palabras
Implementa una función que reciba una frase y cambie el orden de las palabras usando slicing.

## 7. Decorador de Mayúsculas
Escribe un decorador que convierta el resultado de una función en mayúsculas si es string.

## 8. Convertidor de Tipos
Dado un diccionario, crea una función que devuelva un nuevo diccionario donde cada valor sea reemplazado por su tipo.

## 9. Concatenador Inteligente
Crea una función que reciba varios strings y devuelva uno solo, concatenado usando `.join()`, eliminando espacios extra.

## 10. Verificador de Primos para Strings
Implementa una función que reciba un número en string y determine si es primo sin convertirlo completamente a int.

## 11. Contador de Dígitos
Haz una función que cuente cuántas veces aparece cada digito del 0 al 9 en un string.

## 12. Generador de Substrings
Crea un generador que, dado un string, devuelva sus substrings consecutivos de tamaño fijo.

## 13. Buscador de Archivos Grandes
Usa `pathlib` para listar solo los archivos mayores de cierto tamaño.

## 14. Multiplicador por Índice
Implementa una función que reciba una lista de números y regrese otra lista con los números multiplicados por su índice.

## 15. Limpiador de Texto
Escribe una función que limpie un texto dejando solo letras y números usando `translate`.

## 16. Normalizador de Vocales
Dado un texto, reemplaza todas las vocales acentuadas por vocales simples, usando `unicodedata`.

## 17. Módulo de Utilidades de Strings
Implementa un módulo con funciones de manipulación de strings: invertir, limpiar espacios, contar símbolos.

## 18. Decorador de Log de Ejecución
Crea un decorador que imprima el nombre de la función antes de ejecutarse.

## 19. Formateador de Números
Escribe una función que convierta un número entero a su forma con comas: `10000 → "10,000"`.

## 20. Detector de Estructuras Análogas
Implementa una función que detecte si dos listas son "análogas": misma forma, sin importar valores.

---

# 🟩 **Nivel Avanzado — Set 3 (20 ejercicios nuevos)**

## 21. Evaluador de Expresiones Matemáticas
Implementa una función que reciba una expresión matemática como string (solo +, -, *, /, paréntesis) y la evalúe sin usar `eval`.

## 22. Decorador de Unificación de Excepciones
Escribe un decorador que convierta todas las excepciones de una función en un único tipo `AppError`.

## 23. Extractor de Etiquetas HTML
Implementa una función que tome un HTML simple como texto y extraiga todas las etiquetas `<tag>` usando regex.

## 24. Analizador de Extensiones
Escribe un analizador que cuente cuántas veces aparece cada extensión dentro de un árbol de directorios con `pathlib.rglob`.

## 25. Convertidor de Casos de Variables
Implementa un sistema que transforme nombres de variables entre `snake_case`, `camelCase` y `PascalCase`.

## 26. Aplanador de JSONs Anidados
Escribe una función que reciba JSONs anidados y aplaste todas las claves en un solo diccionario plano.

## 27. Generador de Fibonacci Eficiente
Implementa un generador que devuelva los n primeros números de Fibonacci sin guardar toda la secuencia en memoria.

## 28. Decorador Analizador de Argumentos
Crea un decorador que imprima cuántos argumentos recibe una función y de qué tipo es cada uno.

## 29. Agrupador por Letra Final
Implementa un algoritmo que agrupe palabras por su letra final usando `defaultdict`.

## 30. Detector de Bloques Repetidos
Escribe una función que identifique bloques repetidos en un texto usando `re.finditer`.

## 31. Validador de Rutas Linux
Implementa un validador de rutas tipo Linux (`../`, `./`, `/home/user/...`) y determina si son rutas absolutas o relativas.

## 32. Filtro de Diccionarios por Claves
Crea una función que tome una lista de diccionarios y devuelva solo aquellos que tengan todas las claves requeridas.

## 33. Lector de CSV Tolerante a Fallos
Implementa un lector de archivos CSV que pueda ignorar líneas corruptas usando excepciones personalizadas.

## 34. Decorador de Conversión a Decimal
Escribe un decorador que convierta valores de retorno numéricos a `Decimal` automáticamente.

## 35. Limitador de Frecuencias
Implementa una función que tome una lista y devuelva una versión donde cada elemento aparece a lo sumo 2 veces.

## 36. Validador de Autómata Simple
Escribe un analizador que determine si un texto puede ser palabra válida en un autómata simple (regex + lógica).

## 37. Detector de Ciclos en Estructuras
Implementa un verificador que detecte ciclos en estructuras dict anidadas (como grafos).

## 38. Divisor de Texto en Oraciones
Crea una función que divida texto en oraciones usando reglas básicas (puntos, signos, saltos de línea).

## 39. Combinador de Archivos sin Duplicados
Implementa un sistema que combine varios archivos `.txt` en uno solo, sin duplicar líneas repetidas.

## 40. Analizador de Frecuencia de Bytes
Escribe una función que lea un archivo binario y produzca estadísticas del byte más frecuente.

---

# 🟥 **Nivel Competitivo — Set 3 (20 ejercicios nuevos)**

## 41. Buscador de Substring Prefijo Más Largo
Dado un string, encuentra el substring más largo que es también un prefijo del string.

## 42. Calculador de Distancia de Edición
Calcula el número mínimo de operaciones para convertir un string en otro usando solo inserciones y borrados.

## 43. Buscador de Pares con XOR Mínimo
Dado un array, encuentra el número de pares `(i, j)` tales que `i < j` y `arr[i] XOR arr[j]` es mínimo.

## 44. Implementación de Búsqueda Ternaria
Implementa el algoritmo de búsqueda ternaria para encontrar el mínimo de una función unimodal simulada.

## 45. Buscador de Carácter Más Frecuente Izquierdo
Dado un string, encuentra el carácter que aparece con mayor frecuencia *y* cuya última aparición esté más a la izquierda.

## 46. Verificador de Reorganización para Palíndromo
Crea un algoritmo que determine si un string puede reorganizarse para formar un palíndromo.

## 47. Contador de Subarreglos por Producto
Dado un arreglo de enteros, encuentra la cantidad de subarreglos cuyo producto es menor que un valor K.

## 48. Algoritmo de Kadane con Subarreglo
Implementa el algoritmo de Kadane para encontrar la máxima suma, pero también devuelve el subarreglo exacto.

## 49. Calculador de Cortes para Palíndromos
Dado un string, determina el número mínimo de cortes necesarios para dividirlo en substrings palíndromos.

## 50. Contador de Subsecuencias Ascendentes
Calcula el número de subsecuencias estrictamente ascendentes en un arreglo de longitud N.

## 51. Buscador de Centro de Arreglo
Encuentra el "centro" de un arreglo: el índice donde la suma a la izquierda y derecha sea igual.

## 52. Calculador de Frecuencia Máxima en Ventana
Dado un string, halla la frecuencia máxima de cualquier letra dentro de cualquier substring de longitud K.

## 53. Buscador de Índice en Array Rotado
Dado un arreglo rotado, encuentra el índice donde inicia el arreglo original usando binary search.

## 54. Verificador de Patrón Valle
Implementa un algoritmo que determine si una palabra es "valle": desciende y luego asciende.

## 55. Calculador de Suma Máxima No Consecutiva
Resuelve la suma máxima de 3 elementos no consecutivos en un arreglo.

## 56. Contador de Substrings Únicos
Dado un string, encuentra el número total de substrings distintas (usa sets y slicing eficiente).

## 57. Multiplicador de Enteros Grandes
Implementa multiplicación manual de dos enteros grandes dados como strings.

## 58. Buscador de Subsecuencia Alternante
Dado un arreglo, encuentra la subsecuencia más larga donde la paridad vaya alternando estrictamente.

## 59. Verificador de Orden con Un Swap
Verifica si un arreglo puede convertirse en uno ordenado con a lo sumo un swap.

## 60. Optimizador de Frecuencias Únicas
Dado un string, elimina el número mínimo de caracteres para que todas las frecuencias sean diferentes entre sí.