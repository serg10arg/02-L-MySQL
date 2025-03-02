### Actualización de Datos

Cuando se añadieron inicialmente los datos para William Turner a la tabla, los datos de las diversas columnas de dirección no se incluyeron en la sentencia `INSERT`. La siguiente sentencia muestra cómo estas columnas pueden poblarse más tarde mediante una sentencia `UPDATE`:

```sql
mysql> UPDATE person
    -> SET street = '1225 Tremont St.',
    ->     city = 'Boston',
    ->     state = 'MA',
    ->     country = 'USA',
    ->     postal_code = '02138'
    -> WHERE person_id = 1;
```

**Resultado**:
```
Query OK, 1 row affected (0.04 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

El servidor respondió con un mensaje de dos líneas:
- **Rows matched: 1**: Indica que la condición en la cláusula `WHERE` coincidió con una sola fila en la tabla.
- **Changed: 1**: Indica que una sola fila en la tabla ha sido modificada.

Dado que la cláusula `WHERE` especifica la clave primaria de la fila de William, esto es exactamente lo que esperas que suceda.

### Modificación de Múltiples Filas

Dependiendo de las condiciones en tu cláusula `WHERE`, también es posible modificar más de una fila usando una sola sentencia. Por ejemplo, considera lo que sucedería si tu cláusula `WHERE` se viera así:
```sql
WHERE person_id < 10
```

Dado que tanto William como Susan tienen un valor de `person_id` menor que 10, ambas filas serían modificadas. Si dejas fuera la cláusula `WHERE` por completo, tu sentencia `UPDATE` modificará todas las filas en la tabla.

### Ejemplo Completo de la Sentencia `UPDATE`

Supongamos que queremos actualizar los datos de dirección para William Turner:

```sql
UPDATE person
SET street = '1225 Tremont St.',
    city = 'Boston',
    state = 'MA',
    country = 'USA',
    postal_code = '02138'
WHERE person_id = 1;
```

Este comando actualizará la dirección de William Turner en la tabla `person` para reflejar los nuevos datos.

### Resumen

- **Uso de la Cláusula `WHERE`**: Especifica qué fila o filas se actualizarán.
- **Resultado**: El servidor proporciona retroalimentación sobre cuántas filas coinciden con la condición y cuántas filas fueron modificadas.
- **Actualización de Múltiples Filas**: Puede modificar varias filas si la condición `WHERE` coincide con más de una fila.

