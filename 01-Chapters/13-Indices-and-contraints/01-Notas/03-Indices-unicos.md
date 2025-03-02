### Índices Únicos

Los índices únicos son una herramienta poderosa en SQL que no solo mejoran el rendimiento de las consultas, sino que también garantizan la unicidad de los valores en una columna específica. Esto es especialmente útil para evitar duplicados en columnas críticas, como correos electrónicos en una tabla de clientes.

### Eliminación de Índices

Antes de entrar en los detalles de los índices únicos, vale la pena mencionar cómo eliminar un índice en diferentes sistemas de bases de datos.

### SQL Server y Oracle Database

En SQL Server y Oracle Database, se utiliza el comando `DROP INDEX` para eliminar un índice:

```sql
-- Oracle Database
DROP INDEX idx_email;

-- SQL Server
DROP INDEX idx_email ON customer;

```

### MySQL

MySQL también admite el comando `DROP INDEX`, aunque está mapeado al comando `ALTER TABLE`:

```sql
ALTER TABLE customer
DROP INDEX idx_email;

```

### Índices Únicos

Cuando diseñes una base de datos, es crucial decidir qué columnas deben permitir valores duplicados y cuáles no. Por ejemplo, es común tener varios clientes con el mismo nombre en una tabla `customer`, ya que cada fila tiene un identificador único (`customer_id`), correo electrónico y dirección para diferenciarlos. Sin embargo, no querrías que dos clientes diferentes tengan la misma dirección de correo electrónico. Para evitar esto, puedes crear un índice único en la columna `customer.email`.

### Creación de Índices Únicos

Un índice único no solo proporciona los beneficios de un índice regular (mejora el rendimiento de las consultas) sino que también garantiza que no haya valores duplicados en la columna indexada. Cada vez que se inserta una fila o se modifica la columna indexada, el servidor de la base de datos verifica el índice único para asegurarse de que el valor no exista ya en otra fila de la tabla.

### MySQL

Para crear un índice único en MySQL, se utiliza el comando `ALTER TABLE` con la cláusula `ADD UNIQUE`:

```sql
ALTER TABLE customer
ADD UNIQUE idx_email (email);

```

### SQL Server y Oracle Database

En SQL Server y Oracle Database, se utiliza el comando `CREATE UNIQUE INDEX`:

```sql
-- Oracle Database
CREATE UNIQUE INDEX idx_email
ON customer (email);

-- SQL Server
CREATE UNIQUE INDEX idx_email
ON customer (email);

```

### Comportamiento de los Índices Únicos

Con un índice único en su lugar, cualquier intento de insertar un valor duplicado en la columna indexada resultará en un error. Por ejemplo:

```sql
INSERT INTO customer
(store_id, first_name, last_name, email, address_id, active)
VALUES
(1, 'ALAN', 'KAHN', 'ALAN.KAHN@sakilacustomer.org', 394, 1);

```

Si ya existe una fila con el correo electrónico `ALAN.KAHN@sakilacustomer.org`, el servidor de la base de datos lanzará un error:

```
ERROR 1062 (23000): Duplicate entry 'ALAN.KAHN@sakilacustomer.org' for key 'idx_email'

```

### Consideraciones Importantes

- **Clave Primaria**: No es necesario crear un índice único en la columna de clave primaria, ya que el servidor ya garantiza la unicidad de los valores de la clave primaria.
- **Múltiples Índices Únicos**: Puedes crear más de un índice único en la misma tabla si es necesario. Esto puede ser útil para garantizar la unicidad en varias columnas críticas.

### Resumen

- **Eliminación de Índices**: En SQL Server y Oracle Database, se usa `DROP INDEX`. En MySQL, se usa `ALTER TABLE ... DROP INDEX`.
- **Creación de Índices Únicos**: En MySQL, se usa `ALTER TABLE ... ADD UNIQUE`. En SQL Server y Oracle Database, se usa `CREATE UNIQUE INDEX`.
- **Función de los Índices Únicos**: Mejoran el rendimiento de las consultas y garantizan la unicidad de los valores en la columna indexada.
- **Errores de Duplicados**: Cualquier intento de insertar un valor duplicado en una columna con un índice único resultará en un error.

Los índices únicos son una herramienta esencial para mantener la integridad de los datos y mejorar el rendimiento de las consultas en tus bases de datos.