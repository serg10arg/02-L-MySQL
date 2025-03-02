
**Objetivo:**

Queremos añadir una restricción de clave foránea a la tabla `rental` que haga referencia a la tabla `customer`. Esta restricción debe configurarse de manera que, si se intenta eliminar una fila de la tabla `customer` que tenga un `customer_id` que esté siendo utilizado en la tabla `rental`, se genere un error. En otras palabras, necesitamos implementar el comportamiento `ON DELETE RESTRICT`.

**Sentencia `ALTER TABLE`:**

Aquí está la sentencia `ALTER TABLE` que logrará el objetivo:

```sql
ALTER TABLE rental
ADD CONSTRAINT fk_rental_customer  -- Nombre de la restricción (puedes elegir otro nombre descriptivo)
FOREIGN KEY (customer_id)         -- Columna en la tabla 'rental' que será la clave foránea
REFERENCES customer (customer_id)  -- Tabla 'customer' y columna 'customer_id' a la que hace referencia
ON DELETE RESTRICT;              -- Comportamiento 'ON DELETE RESTRICT'
```

**Explicación Detallada de la Sentencia:**

*   **`ALTER TABLE rental`**:  Esta parte indica que vamos a modificar la estructura de la tabla `rental`.
*   **`ADD CONSTRAINT fk_rental_customer`**:  Esto añade una nueva restricción a la tabla `rental`.  `fk_rental_customer` es el nombre que le estamos dando a esta restricción. Es una buena práctica nombrar las restricciones de manera descriptiva para que sea más fácil identificarlas y gestionarlas posteriormente.  Puedes elegir otro nombre si lo prefieres, por ejemplo, `rental_customer_fk_constraint`.
*   **`FOREIGN KEY (customer_id)`**:  Esto especifica que estamos creando una restricción de clave foránea.  `(customer_id)` indica que la columna `customer_id` de la tabla `rental` será la clave foránea.
*   **`REFERENCES customer (customer_id)`**:  Esto define la tabla y la columna a la que la clave foránea hace referencia.  `customer` es la tabla padre, y `customer_id` es la columna de la tabla `customer` que es la clave primaria (o una clave única) y a la que `rental.customer_id` hará referencia.
*   **`ON DELETE RESTRICT`**: Esta es la parte crucial para cumplir con el requisito del ejercicio.  `ON DELETE RESTRICT`  especifica que si se intenta eliminar una fila de la tabla `customer` cuyo `customer_id` está referenciado por al menos una fila en la tabla `rental` (en la columna `customer_id`), la operación de eliminación en la tabla `customer` **fallará** y se generará un error.  Esto asegura la integridad referencial, evitando que existan registros "huérfanos" en la tabla `rental` que referencien a clientes que ya no existen en la tabla `customer`.


Después de ejecutar esta sentencia, la restricción de clave foránea `fk_rental_customer` estará activa en la tabla `rental`, y el comportamiento `ON DELETE RESTRICT` se aplicará para las eliminaciones en la tabla `customer` que involucren `customer_id` referenciados en `rental`.

