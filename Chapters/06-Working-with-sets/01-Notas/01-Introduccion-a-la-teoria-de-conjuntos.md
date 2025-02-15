
---

# Trabajar con Conjuntos

Aunque puedes interactuar con los datos en una base de datos una fila a la vez, las bases de datos relacionales realmente se tratan de conjuntos. Este capítulo explora cómo puedes combinar múltiples conjuntos de resultados utilizando varios operadores de conjuntos. Después de una breve visión general de la teoría de conjuntos, demostraré cómo usar los operadores de conjuntos `union`, `intersect` y `except` para combinar múltiples conjuntos de datos.

# Introducción a la Teoría de Conjuntos

En muchas partes del mundo, la teoría básica de conjuntos se incluye en los currículos de matemáticas a nivel elemental. Tal vez recuerdes haber visto algo como lo que se muestra en la Figura 6-1.

## Figura 6-1. La operación de unión

La área sombreada en la Figura 6-1 representa la unión de los conjuntos A y B, que es la combinación de los dos conjuntos (con cualquier región superpuesta incluida solo una vez). ¿Te empieza a sonar familiar? Si es así, finalmente tendrás la oportunidad de poner ese conocimiento en uso; si no, no te preocupes, porque es fácil de visualizar usando un par de diagramas.

Usando círculos para representar dos conjuntos de datos (A y B), imagina un subconjunto de datos que es común a ambos conjuntos; estos datos comunes están representados por el área superpuesta mostrada en la Figura 6-1. Dado que la teoría de conjuntos es bastante poco interesante sin una superposición entre conjuntos de datos, uso el mismo diagrama para ilustrar cada operación de conjunto. Hay otra operación de conjunto que se ocupa solo de la superposición entre dos conjuntos de datos; esta operación se conoce como la intersección y se demuestra en la Figura 6-2.

## Figura 6-2. La operación de intersección

El conjunto de datos generado por la intersección de los conjuntos A y B es solo el área de superposición entre los dos conjuntos. Si los dos conjuntos no tienen superposición, entonces la operación de intersección produce el conjunto vacío.

La tercera y última operación de conjunto, que se demuestra en la Figura 6-3, se conoce como la operación de excepción.

## Figura 6-3. La operación de excepción

La Figura 6-3 muestra los resultados de A excepto B, que es la totalidad del conjunto A menos cualquier superposición con el conjunto B. Si los dos conjuntos no tienen superposición, entonces la operación A excepto B produce la totalidad del conjunto A.

Usando estas tres operaciones, o combinando diferentes operaciones, puedes generar los resultados que necesites. Por ejemplo, imagina que quieres construir un conjunto como el demostrado en la Figura 6-4.

## Figura 6-4. Conjunto de datos misterioso

El conjunto de datos que buscas incluye todos los conjuntos A y B sin la región superpuesta. No puedes lograr este resultado con solo una de las tres operaciones mostradas anteriormente; en su lugar, necesitarás primero construir un conjunto de datos que abarque todos los conjuntos A y B, y luego utilizar una segunda operación para eliminar la región superpuesta. Si el conjunto combinado se describe como A unión B, y la región superpuesta se describe como A intersección B, entonces la operación necesaria para generar el conjunto de datos representado por la Figura 6-4 sería la siguiente:

```
(A unión B) excepto (A intersección B)
```

Por supuesto, a menudo hay múltiples maneras de lograr los mismos resultados; podrías alcanzar un resultado similar utilizando la siguiente operación:

```
(A excepto B) unión (B excepto A)
```

Aunque estos conceptos son bastante fáciles de entender usando diagramas, las próximas secciones mostrarán cómo se aplican estos conceptos a una base de datos relacional utilizando los operadores de conjuntos de SQL.

---

