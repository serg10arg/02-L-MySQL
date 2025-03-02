### Evaluación de Condiciones en la Cláusula `WHERE`

Una cláusula `WHERE` puede contener una o más condiciones, separadas por los operadores `AND` y `OR`. La evaluación de las condiciones determina si una fila se incluye o no en el conjunto de resultados.

#### Uso del Operador `AND`

Cuando las condiciones están separadas por el operador `AND`, todas las condiciones deben evaluarse como verdaderas para que la fila se incluya en el conjunto de resultados. Considera la siguiente cláusula `WHERE`:

```sql
WHERE first_name = 'STEVEN' AND create_date > '2006-01-01'
```

Dadas estas dos condiciones, solo se incluirán en el conjunto de resultados las filas donde el `first_name` sea 'STEVEN' y la fecha de creación (`create_date`) sea posterior al 1 de enero de 2006. No importa cuántas condiciones haya en tu cláusula `WHERE`, si están separadas por el operador `AND`, todas deben evaluarse como verdaderas para que la fila se incluya en el conjunto de resultados.

#### Uso del Operador `OR`

Si todas las condiciones en la cláusula `WHERE` están separadas por el operador `OR`, solo una de las condiciones debe evaluarse como verdadera para que la fila se incluya en el conjunto de resultados. Considera las siguientes dos condiciones:

```sql
WHERE first_name = 'STEVEN' OR create_date > '2006-01-01'
```

Ahora hay varias formas en que una fila puede ser incluida en el conjunto de resultados:
- El `first_name` es 'STEVEN' y la `create_date` fue posterior al 1 de enero de 2006.
- El `first_name` es 'STEVEN' y la `create_date` fue en o antes del 1 de enero de 2006.
- El `first_name` es cualquier cosa que no sea 'STEVEN', pero la `create_date` fue posterior al 1 de enero de 2006.

#### Evaluación de Dos Condiciones con `OR`

La tabla siguiente muestra los posibles resultados para una cláusula `WHERE` que contiene dos condiciones separadas por el operador `OR`:

| Resultado Intermedio | Resultado Final |
|----------------------|-----------------|
| WHERE true OR true   | true            |
| WHERE true OR false  | true            |
| WHERE false OR true  | true            |
| WHERE false OR false | false           |

En el caso del ejemplo anterior, la única forma en que una fila puede ser excluida del conjunto de resultados es si el nombre de la persona no era 'STEVEN' y la fecha de creación fue en o antes del 1 de enero de 2006.

