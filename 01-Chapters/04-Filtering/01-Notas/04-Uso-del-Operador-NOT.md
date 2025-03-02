### Uso del Operador `NOT`

El operador `NOT` se utiliza para invertir el resultado de una condición. Veamos un ejemplo modificado de una cláusula `WHERE` que incluye el operador `NOT`.

#### Ejemplo de Cláusula `WHERE` con `NOT`

```sql
WHERE NOT (first_name = 'STEVEN' OR last_name = 'YOUNG')
  AND create_date > '2006-01-01'
```

### Desglose del Ejemplo

1. **Operador `NOT`**:
    - El operador `NOT` invierte el resultado de la condición dentro de los paréntesis. En lugar de buscar personas con el nombre 'STEVEN' o el apellido 'YOUNG' cuya fecha de creación sea posterior al 1 de enero de 2006, buscamos filas donde el nombre **no** sea 'STEVEN' **o** el apellido **no** sea 'YOUNG' cuya fecha de creación sea posterior al 1 de enero de 2006.

2. **Condiciones**:
    - `(first_name = 'STEVEN' OR last_name = 'YOUNG')`: Esta condición original busca nombres 'STEVEN' o apellidos 'YOUNG'.
    - `NOT (first_name = 'STEVEN' OR last_name = 'YOUNG')`: Con el operador `NOT`, buscamos filas donde el nombre **no** sea 'STEVEN' y el apellido **no** sea 'YOUNG'.
    - `AND create_date > '2006-01-01'`: Esta condición debe evaluarse como verdadera para que la fila se incluya en el conjunto de resultados.

### Evaluación de Tres Condiciones con `AND`, `OR` y `NOT`

La siguiente tabla muestra los posibles resultados para esta cláusula `WHERE`:

| Resultado Intermedio                     | Resultado Final |
|------------------------------------------|-----------------|
| WHERE NOT (true OR true) AND true        | false           |
| WHERE NOT (true OR false) AND true       | false           |
| WHERE NOT (false OR true) AND true       | false           |
| WHERE NOT (false OR false) AND true      | true            |
| WHERE NOT (true OR true) AND false       | false           |
| WHERE NOT (true OR false) AND false      | false           |
| WHERE NOT (false OR true) AND false      | false           |
| WHERE NOT (false OR false) AND false     | false           |

Como puedes ver, el uso del operador `NOT` puede complicar la evaluación de las condiciones. Para hacer más fácil de entender la cláusula `WHERE`, puedes reescribirla sin usar el operador `NOT`:

```sql
WHERE first_name <> 'STEVEN' AND last_name <> 'YOUNG'
  AND create_date > '2006-01-01'
```

### Conclusión

Si bien el servidor de bases de datos puede manejar fácilmente el operador `NOT`, a menudo es difícil para una persona evaluar una cláusula `WHERE` que lo incluye. Por eso, no es muy común encontrarlo en las consultas. Reescribir la cláusula sin el operador `NOT` puede hacer que sea más fácil de entender.

