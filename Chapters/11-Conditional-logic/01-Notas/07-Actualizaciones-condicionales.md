### Actualizaciones Condicionales

Las actualizaciones condicionales en SQL son útiles cuando necesitas actualizar filas en una tabla basándote en ciertas condiciones. En el ejemplo proporcionado, se actualiza la columna `active` de la tabla `customer` según la actividad reciente del cliente.

### Ejemplo de Actualización Condicional

#### Escenario

Supongamos que tienes una tabla `customer` que contiene información sobre los clientes, incluyendo una columna `active` que indica si el cliente está activo (1) o inactivo (0). Además, tienes una tabla `rental` que contiene información sobre los alquileres de películas, incluyendo la fecha de alquiler (`rental_date`) y el identificador del cliente (`customer_id`).

#### Consulta de Actualización Condicional

```sql
UPDATE customer
SET active =
  CASE
    WHEN 90 <= (SELECT datediff(now(), max(rental_date))
                FROM rental r
                WHERE r.customer_id = customer.customer_id)
      THEN 0
    ELSE 1
  END
WHERE active = 1;
```

#### Explicación Detallada

1. **Selección de Filas a Actualizar**:
    - `UPDATE customer`: Indica que se va a actualizar la tabla `customer`.
    - `WHERE active = 1`: Filtra las filas para actualizar solo aquellos clientes que actualmente están activos (es decir, `active = 1`).

2. **Lógica Condicional para Actualizar la Columna `active`**:
    - `SET active = CASE ... END`: Utiliza una expresión `CASE` para determinar el nuevo valor de la columna `active`.
    - `WHEN 90 <= (SELECT datediff(now(), max(rental_date)) FROM rental r WHERE r.customer_id = customer.customer_id) THEN 0`:
        - `SELECT datediff(now(), max(rental_date)) FROM rental r WHERE r.customer_id = customer.customer_id`:
            - `datediff(now(), max(rental_date))`: Calcula el número de días entre la fecha actual (`now()`) y la última fecha de alquiler (`max(rental_date)`) para el cliente.
            - `FROM rental r WHERE r.customer_id = customer.customer_id`: Filtra las filas de la tabla `rental` para obtener solo las alquileres del cliente actual.
        - `WHEN 90 <= ... THEN 0`: Si el número de días desde la última fecha de alquiler es 90 o más, el cliente se marca como inactivo (`active = 0`).
    - `ELSE 1`: Si el número de días es menos de 90, el cliente se mantiene activo (`active = 1`).

#### Resultado

La consulta actualiza la columna `active` de la tabla `customer` de la siguiente manera:
- Si el cliente no ha alquilado una película en los últimos 90 días, `active` se establece en 0.
- Si el cliente ha alquilado una película en los últimos 90 días, `active` se mantiene en 1.

### Consideraciones Importantes

1. **Eficiencia**:
    - La subconsulta correlacionada puede ser costosa en términos de rendimiento, especialmente si la tabla `rental` es grande. En bases de datos grandes, considera optimizar la consulta o utilizar índices adecuados.

2. **Transacciones**:
    - Si es necesario, puedes envolver esta actualización en una transacción para asegurarte de que se realice correctamente y de manera consistente.

3. **Pruebas**:
    - Antes de ejecutar la actualización en producción, es recomendable probar la consulta en un entorno de desarrollo para asegurarte de que funciona como se espera.

### Ejemplo de Prueba

Para probar la lógica sin actualizar la tabla, puedes ejecutar una consulta `SELECT` similar:

```sql
SELECT c.customer_id, c.first_name, c.last_name, c.active,
       CASE
         WHEN 90 <= (SELECT datediff(now(), max(rental_date))
                     FROM rental r
                     WHERE r.customer_id = c.customer_id)
           THEN 0
         ELSE 1
       END AS new_active
FROM customer c
WHERE c.active = 1;
```

Esta consulta mostrará el estado actual y el nuevo estado de la columna `active` sin realizar cambios en la tabla.

### Resumen

- **Actualización Condicional**: Utiliza una expresión `CASE` para actualizar una columna basada en una condición.
- **Subconsulta Correlacionada**: Calcula el número de días desde la última fecha de alquiler para cada cliente.
- **Condición de Actualización**: Solo se actualizan los clientes que actualmente están activos (`active = 1`).

