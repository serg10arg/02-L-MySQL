### Uso de Paréntesis en la Cláusula `WHERE`

Cuando tu cláusula `WHERE` incluye tres o más condiciones utilizando los operadores `AND` y `OR`, deberías usar paréntesis para aclarar tu intención tanto para el servidor de la base de datos como para cualquier otra persona que lea tu código. Esto asegura que las condiciones se evalúen en el orden que esperas.

#### Ejemplo de Cláusula `WHERE` con Paréntesis

Aquí tienes una cláusula `WHERE` que amplía el ejemplo anterior al verificar que el nombre sea Steven o el apellido sea Young, y que la fecha de creación sea posterior al 1 de enero de 2006:

```sql
WHERE (first_name = 'STEVEN' OR last_name = 'YOUNG')
  AND create_date > '2006-01-01'
```

Ahora hay tres condiciones; para que una fila se incluya en el conjunto de resultados, la primera o la segunda condición (o ambas) deben evaluarse como verdaderas, y la tercera condición debe evaluarse como verdadera.

#### Evaluación de Tres Condiciones con `AND` y `OR`

La siguiente tabla muestra los posibles resultados para esta cláusula `WHERE`:

| Resultado Intermedio             | Resultado Final |
|----------------------------------|-----------------|
| WHERE (true OR true) AND true    | true            |
| WHERE (true OR false) AND true   | true            |
| WHERE (false OR true) AND true   | true            |
| WHERE (false OR false) AND true  | false           |
| WHERE (true OR true) AND false   | false           |
| WHERE (true OR false) AND false  | false           |
| WHERE (false OR true) AND false  | false           |
| WHERE (false OR false) AND false | false           |

Como puedes ver, a medida que aumentas el número de condiciones en tu cláusula `WHERE`, hay más combinaciones para que el servidor las evalúe. En este caso, solo tres de las ocho combinaciones producen un resultado final de verdadero.

### Conclusión

Usar paréntesis en tu cláusula `WHERE` es crucial cuando mezclas diferentes operadores (`AND` y `OR`) para asegurarte de que las condiciones se evalúen correctamente y el resultado sea el esperado. Esto también facilita la comprensión de tu intención para cualquier persona que lea tu código más adelante.

