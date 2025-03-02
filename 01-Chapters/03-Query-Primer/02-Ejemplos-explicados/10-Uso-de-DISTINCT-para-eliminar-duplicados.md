
### **Consulta SQL**

```sql
SELECT DISTINCT customer_id
FROM rental
WHERE date(rental_date) = '2005-07-05';
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT DISTINCT`**
```sql
SELECT DISTINCT customer_id
```
- **`SELECT`**: Indica que queremos seleccionar datos de la base de datos.
- **`DISTINCT`**: Esta palabra clave asegura que los resultados no contengan duplicados. En este caso, solo se devolverán valores únicos de `customer_id`.
- **`customer_id`**: Es la columna que estamos seleccionando. Contiene el identificador único de cada cliente.

---

#### 2. **`FROM`**
```sql
FROM rental
```
- Especificamos que estamos trabajando con la tabla `rental`, que contiene información sobre los alquileres realizados por los clientes.

---

#### 3. **`WHERE`**
```sql
WHERE date(rental_date) = '2005-07-05'
```
- **`date(rental_date)`**: La función `date()` extrae solo la parte de la fecha de la columna `rental_date` (que probablemente contiene una fecha y hora completa).
- **`= '2005-07-05'`**: Filtramos los resultados para incluir solo los alquileres que ocurrieron en la fecha `5 de julio de 2005`.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Filtra por fecha**: Solo se incluyen los alquileres que ocurrieron el `5 de julio de 2005`.
2. **Selecciona `customer_id`**: Obtenemos los identificadores de los clientes que realizaron alquileres en esa fecha.
3. **Elimina duplicados**: Usamos `DISTINCT` para asegurarnos de que cada `customer_id` aparezca solo una vez en los resultados, incluso si un cliente realizó múltiples alquileres en esa fecha.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `rental`:

| rental_id | customer_id | rental_date        |
|-----------|-------------|--------------------|
| 1         | 101         | 2005-07-05 10:30:00|
| 2         | 102         | 2005-07-05 12:45:00|
| 3         | 101         | 2005-07-05 15:00:00|
| 4         | 103         | 2005-07-06 09:00:00|
| 5         | 102         | 2005-07-05 18:30:00|

---

#### Resultado de la Consulta
La consulta devolverá:

| customer_id |
|-------------|
| 101         |
| 102         |

- **Explicación**:
  - Los alquileres con `rental_id = 1`, `2`, `3` y `5` ocurrieron el `5 de julio de 2005`.
  - Los `customer_id` involucrados son `101` y `102`.
  - Aunque el cliente `101` realizó dos alquileres y el cliente `102` realizó dos alquileres, el uso de `DISTINCT` asegura que cada `customer_id` aparezca solo una vez en los resultados.

---

### **Resumen**

- **`SELECT DISTINCT`**: Selecciona valores únicos de `customer_id`.
- **`FROM rental`**: Especifica que estamos trabajando con la tabla `rental`.
- **`WHERE`**: Filtra los resultados para incluir solo los alquileres del `5 de julio de 2005`.

