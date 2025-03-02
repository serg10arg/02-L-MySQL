
### 1. **Creación de una Vista (`CREATE VIEW`)**

```sql
CREATE VIEW cust_vw AS
SELECT customer_id, first_name, last_name, active
FROM customer;
```

- **`CREATE VIEW cust_vw AS`**: Esta parte de la sentencia está creando una **vista** llamada `cust_vw`. Una vista en SQL es como una "tabla virtual" que se basa en el resultado de una consulta `SELECT`. No almacena datos físicamente, sino que simplemente guarda la consulta para que puedas usarla más tarde como si fuera una tabla.

- **`SELECT customer_id, first_name, last_name, active FROM customer;`**: Esta es la consulta que define la vista. Aquí, estamos seleccionando cuatro columnas de la tabla `customer`: `customer_id`, `first_name`, `last_name`, y `active`. La vista `cust_vw` contendrá estos datos.

- **`Query OK, 0 rows affected (0.12 sec)`**: Esto es un mensaje que indica que la vista se ha creado correctamente. No se han modificado filas en ninguna tabla, simplemente se ha creado la vista.

### 2. **Consulta a la Vista (`SELECT`)**

```sql
SELECT first_name, last_name
FROM cust_vw
WHERE active = 0;
```

- **`SELECT first_name, last_name`**: Aquí estamos seleccionando dos columnas: `first_name` y `last_name`. Estas columnas provienen de la vista `cust_vw` que acabamos de crear.

- **`FROM cust_vw`**: Esto indica que estamos consultando la vista `cust_vw`, que a su vez obtiene sus datos de la tabla `customer`.

- **`WHERE active = 0`**: Esta es una condición que filtra los resultados. Solo se seleccionarán las filas donde la columna `active` sea igual a `0`. En este contexto, `active = 0` podría significar que el cliente está inactivo.

### **¿Qué hace esta consulta en conjunto?**

1. **Creación de la Vista**: Primero, se crea una vista llamada `cust_vw` que contiene los datos de `customer_id`, `first_name`, `last_name`, y `active` de la tabla `customer`.

2. **Consulta a la Vista**: Luego, se realiza una consulta a la vista `cust_vw` para obtener los nombres (`first_name` y `last_name`) de los clientes que están inactivos (`active = 0`).

### **Ejemplo Práctico**

Imagina que tienes una tabla `customer` con los siguientes datos:

| customer_id | first_name | last_name | active |
|-------------|------------|-----------|--------|
| 1           | John       | Doe       | 1      |
| 2           | Jane       | Smith     | 0      |
| 3           | Alice      | Johnson   | 0      |

- **Vista `cust_vw`**: La vista `cust_vw` contendrá los mismos datos que la tabla `customer`, pero solo las columnas seleccionadas.

- **Consulta `SELECT`**: Cuando ejecutas la consulta `SELECT first_name, last_name FROM cust_vw WHERE active = 0;`, obtendrás:

| first_name | last_name |
|------------|-----------|
| Jane       | Smith     |
| Alice      | Johnson   |

Esto se debe a que solo Jane y Alice tienen `active = 0`, lo que significa que están inactivas.

### **Resumen**

- **Vista (`CREATE VIEW`)**: Es una forma de guardar una consulta como si fuera una tabla virtual.
- **Consulta a la Vista (`SELECT`)**: Puedes consultar la vista como si fuera una tabla normal, aplicando condiciones como `WHERE`.

