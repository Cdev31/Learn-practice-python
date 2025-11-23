# 🟦 **Nivel Intermedio — 20 Ejercicios Combinados (Set 2)**

## 1. Normalizador de Texto
Crea una función que reciba un texto y devuelva otro string con cada palabra capitalizada, eliminando tildes y caracteres no alfabéticos.

## 2. Contador de Tipos
Escribe una función que reciba una lista de valores y devuelva un diccionario con cuántos son `str`, `int`, `float`, `bool`, y "otros".

## 3. Filtro de Números Válidos
Implementa una función que reciba una lista de números (pueden venir como strings) y devuelva solo los válidos como enteros, usando excepciones.

## 4. Convertidor a Formato Guiones
Haz una función que convierta una frase a un formato donde cada palabra esté separada por guiones y sin espacios repetidos.

## 5. Buscador de Archivos por Fecha
Usando `pathlib`, escribe una función que reciba una ruta y devuelva todos los archivos `.txt` ordenados por fecha de modificación.

## 6. Validador de Nombres de Usuario
Construye una función que valide nombres de usuario:
- solo minúsculas
- longitud entre 3 y 12
- sin dígitos al inicio
Si falla, lanza `ValueError`.

## 7. Contador de Caracteres ASCII
Escribe una función que dado un texto, cuente cuántos caracteres son ASCII y cuántos no usando `ord()`.

## 8. Filtro de Strings Alfanuméricos
Implementa una función que reciba una lista de strings y elimine aquellos que contengan caracteres no alfanuméricos.

## 9. Conversor de Tiempo a Segundos
Crea una función que convierta una cadena con formato `"12h 35m 10s"` en total de segundos usando `split` y validaciones.

## 10. Suma de Dígitos de String Grande
Dado un número grande como string, devuelve la suma de sus dígitos evitando convertir todo a int directamente.

## 11. Mapeador de Posiciones de Letras
Crea una función que reciba un texto y devuelva un diccionario donde las claves son letras y los valores listas con sus posiciones en el texto.

## 12. Decorador Contador de Llamadas
Implementa un decorador que imprima cuántas veces ha sido llamada una función.

## 13. Generador de Tablas Alineadas
Escribe una función que convierta texto en una tabla con columnas alineadas usando f-strings.

## 14. Conversor de Fechas con Validación
Crea una función que reciba una fecha en formato `"DD/MM/YYYY"` y devuelva una estructura `datetime`, manejando errores.

## 15. Filtro de Líneas Válidas
Dado un archivo con líneas vacías y con espacios, crea una función que retorne solo las líneas con contenido válido.

## 16. Generador Inverso
Implementa un generador que reciba una lista y devuelva sus elementos en orden inverso sin usar `reversed()`.

## 17. Validador de Direcciones IP
Escribe un módulo que valide direcciones IP usando regex de 0 a 255.

## 18. Analizador de Composición de Texto
Implementa una función que convierta un texto en un diccionario de:
`{"vocales": x, "consonantes": y, "digitos": z, "otros": k}`.

## 19. Decorador de Conversión de Excepciones
Crea un decorador que convierta cualquier excepción lanzada por la función en un `RuntimeError` con descripción clara.

## 20. Calculadora de Cuadrados Robusta
Implementa una función que reciba números y devuelva una lista con sus cuadrados, pero ignorando silenciosamente valores invalidos.

---

# 🟩 **Nivel Avanzado — 20 Ejercicios Combinados (Set 2)**

## 21. Normalizador de Texto Avanzado
Construye una función que normalice texto:
- lowercase
- sin espacios repetidos
- sin diacríticos
- sin signos
Usa `unicodedata`.

## 22. Buscador de Palíndromos en Archivos
Implementa una función que lea un archivo grande y devuelva las líneas que son palíndromos, ignorando mayúsculas y símbolos.

## 23. Analizador de Logs
Escribe un analizador de logs donde cada línea tiene formato `[HH:MM:SS] TYPE - mensaje`. Devuelve cuántos de cada tipo hay.

## 24. Decorador Multipropósito
Implementa un decorador que mida tiempo, cachee resultados y registre errores (combinando 3 decoradores en 1).

## 25. Aplanador de Diccionarios Anidados
Construye una función que reciba un diccionario anidado y devuelva todas sus claves aplanadas en una sola lista.

## 26. Sistema de Rutas Básico
Implementa un sistema de rutas básicas tipo framework: `/home`, `/user/<id>`, `/post/<id>/edit`, usando funciones + validaciones + parsing.

## 27. Generador de Índice de Palabras
Escribe una función que reciba un conjunto de archivos y genere un índice único de todas las palabras usadas (normalizadas).

## 28. GroupBy Personalizado
Implementa tu propio `groupby` usando solo diccionarios y ciclos.

## 29. Mezclador de Listas Inteligente
Crea una función que mezcle dos listas de forma alternada, pero si una es más larga, agrega elementos restantes al final.

## 30. Validador de Contraseñas
Implementa un módulo con funciones para validar passwords:
- longitud > 8
- mayúscula
- minúscula
- símbolo
- dígito

## 31. Calculadora de Desviación Estándar
Crea una función que calcule desviación estándar sin usar `statistics`, validando tipos y longitudes.

## 32. Validador de JSON Avanzado
Implementa un algoritmo que reciba un archivo JSON y verifique:
- si las claves están repetidas
- si los valores cumplen tipos específicos
- si faltan claves requeridas

## 33. Buscador de Caracteres Frecuentes
Escribe una función que reciba un texto y devuelva los *N* caracteres más frecuentes sin usar `Counter`.

## 34. Decorador de Conversión a String
Implementa un decorador que fuerce que todos los argumentos se conviertan a `str` antes de llamar la función.

## 35. Generador de Matrices
Crea un generador que recorra matrices (listas de listas) fila por fila.

## 36. Detector de Patrones Repetidos
Implementa un sistema que detecte si un texto tiene patrones repetidos usando `re.findall`.

## 37. Buscador de Intersección Múltiple
Escribe una función que reciba varias listas y devuelva su intersección común sin usar sets directamente.

## 38. Convertidor a Texto Tabulado
Implementa un módulo que convierta una lista de objetos en texto tabulado, leyendo atributos via `__dict__`.

## 39. Sistema de Directorios en Memoria
Crea un árbol de directorios en memoria usando diccionarios anidados, con funciones para insertar, eliminar y listar.

## 40. Validador de Paréntesis Detallado
Implementa un verificador de paréntesis que devuelva en qué índice se rompe el balance, no solo True/False.

---

# 🟥 **Nivel Competitivo — 20 Ejercicios (Set 2)**

## 41. Convertidor a Palíndromo
Dado un string, encuentra la cantidad mínima de cambios para convertirlo en un palíndromo.

## 42. Producto Máximo de Subarreglo
Dado un array grande, encuentra el producto máximo de un subarreglo contiguo (incluyendo negativos).

## 43. Verificador de Subsecuencia
Verifica si un string es subsecuencia de otro en O(n), sin usar slicing excesivo.

## 44. Buscador de Elemento con Frecuencia K
Dado un array con repetidos, encuentra el primer elemento que aparece K veces.

## 45. Calculadora de Beauty Score
Calcula el "beauty score" de un string: suma para cada letra de su distancia a su siguiente aparición.

## 46. Buscador de Mínimo en Array Rotado
Dado un arreglo rotado, encuentra el mínimo usando búsqueda binaria modificada.

## 47. Eliminador de Duplicados Consecutivos
Implementa eliminación de caracteres consecutivos duplicados hasta que no queden duplicados adyacentes.

## 48. Substring con K Caracteres Distintos
Encuentra el substring más largo que contiene a lo sumo K caracteres distintos.

## 49. Ordenador por Complejidad de String
Ordena una lista de strings por su "complejidad" donde complejidad = número de cambios entre vocal/consonante.

## 50. Implementación de Algoritmo KMP
Implementa el algoritmo de *prefix function* (KMP) y úsalo para buscar patrones en un texto.

## 51. Convertidor de Números a Texto
Dado un número entero como string, devuelve su representación textual ("cuatrocientos veinte y tres").

## 52. Buscador de Diferencia Mínima
Encuentra el par de números en un arreglo cuya diferencia absoluta sea mínima.

## 53. Buscador de Prefijo Común Máximo
Dado un conjunto de palabras, encuentra el mayor conjunto donde todas comparten un prefijo común.

## 54. Multiplicador de Matrices
Implementa multiplicación de matrices cuadradas en O(n³) sin librerías externas.

## 55. Reorganizador de Array por Valor
Reordena un arreglo para que cada elemento quede en un índice igual a su valor (si existe), sin usar estructuras adicionales.

## 56. Contador de Substrings Únicos
Calcula el número de substrings únicos en un string usando sets y slicing eficiente.

## 57. Buscador de Substring Palindrómico Más Largo
Encuentra el substring palindrómico más largo usando técnica expand-from-center.

## 58. Contador de Pares con Condición Especial
Dado un arreglo, encuentra cuántos pares `(i, j)` cumplen `arr[i] > 2*arr[j]` usando divide and conquer.

## 59. Implementación de Árbol Binario
Implementa un árbol binario básico con inserción, búsqueda y recorrido inorder usando clases.

## 60. Calculador de Eliminaciones para Frecuencia Par
Encuentra el número mínimo de caracteres a eliminar para que un string tenga todos caracteres con frecuencia par.