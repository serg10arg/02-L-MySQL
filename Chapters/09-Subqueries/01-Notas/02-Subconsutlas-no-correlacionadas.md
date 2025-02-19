
---

### **Tipos de Subconsultas**

Las subconsultas se pueden clasificar en dos tipos principales según su relación con la consulta contenedora:

1. **Subconsultas No Correlacionadas**:
    - Son independientes de la consulta contenedora.
    - Pueden ejecutarse por sí solas sin hacer referencia a la consulta principal.
    - Son las más comunes y suelen devolver un solo valor (subconsultas escalares) o un conjunto de valores.

2. **Subconsultas Correlacionadas**:
    - Dependen de la consulta contenedora.
    - Hacen referencia a columnas de la consulta principal.
    - Se ejecutan una vez por cada fila procesada por la consulta contenedora.

En esta sección, nos enfocaremos en las **subconsultas no correlacionadas**.

---

### **Subconsultas No Correlacionadas**

Una subconsulta no correlacionada es aquella que puede ejecutarse de manera independiente y no depende de la consulta contenedora. Estas subconsultas son muy útiles para filtrar datos, generar valores o construir conjuntos de datos temporales.

---

### **Subconsultas Escalares**

Una **subconsulta escalar** es un tipo de subconsulta no correlacionada que devuelve una sola fila con una sola columna. Este tipo de subconsulta se puede usar en condiciones de igualdad o desigualdad (`=`, `<>`, `<`, `>`, `<=`, `>=`).

#### **Ejemplo de Subconsulta Escalar**

Supongamos que queremos obtener todas las ciudades que **no** están en India. Podemos usar una subconsulta escalar para obtener el `country_id` de India y luego compararlo en la consulta principal:

```sql
SELECT city_id, city
FROM city
WHERE country_id <> (SELECT country_id FROM country WHERE country = 'India');
```

#### **Resultado:**
```sql
+---------+----------------------------+
| city_id | city                       |
+---------+----------------------------+
|       1 | A Corua (La Corua)         |
|       2 | Abha                       |
|       3 | Abu Dhabi                  |
|       4 | Acua                       |
|       5 | Adana                      |
|       6 | Addis Abeba                |
...
|     600 | Ziguinchor                 |
+---------+----------------------------+
540 rows in set (0.02 sec)
```

#### **Explicación:**
1. **Subconsulta**:
    - `SELECT country_id FROM country WHERE country = 'India'`:
        - Devuelve el `country_id` de India (por ejemplo, `44`).

2. **Consulta contenedora**:
    - `SELECT city_id, city FROM city WHERE country_id <> 44`:
        - Devuelve todas las ciudades cuyo `country_id` no sea `44`.

---

### **Errores Comunes con Subconsultas Escalares**

Si una subconsulta escalar devuelve más de una fila, obtendrás un error. Por ejemplo, si modificamos la consulta anterior para que la subconsulta devuelva todos los `country_id` excepto el de India:

```sql
SELECT city_id, city
FROM city
WHERE country_id <> (SELECT country_id FROM country WHERE country <> 'India');
```

#### **Error:**
```
ERROR 1242 (21000): Subquery returns more than 1 row
```

#### **Explicación:**
- La subconsulta devuelve múltiples `country_id` (todos excepto el de India).
- No puedes comparar un solo valor (`country_id`) con un conjunto de valores.

---

### **Solución: Usar Operadores de Conjunto**

Para manejar subconsultas que devuelven múltiples filas, puedes usar operadores como `IN`, `ANY`, `ALL`, etc. Por ejemplo:

```sql
SELECT city_id, city
FROM city
WHERE country_id IN (SELECT country_id FROM country WHERE country <> 'India');
```

#### **Explicación:**
- **`IN`**: Filtra las ciudades cuyo `country_id` esté en la lista devuelta por la subconsulta.
- La subconsulta devuelve todos los `country_id` excepto el de India.
- La consulta contenedora devuelve todas las ciudades que no están en India.

---

### **Resumen de Conceptos Clave**

1. **Subconsultas No Correlacionadas**:
    - Son independientes de la consulta contenedora.
    - Pueden ejecutarse por sí solas.

2. **Subconsultas Escalares**:
    - Devuelven una sola fila con una sola columna.
    - Se pueden usar en condiciones de igualdad o desigualdad.

3. **Errores Comunes**:
    - Si una subconsulta escalar devuelve más de una fila, obtendrás un error.

4. **Operadores de Conjunto**:
    - Usa `IN`, `ANY`, `ALL`, etc., para manejar subconsultas que devuelven múltiples filas.

---

### **Ejemplo Adicional**

Supongamos que queremos obtener los nombres de los clientes que han realizado al menos un pago mayor a $10. Podemos usar una subconsulta no correlacionada con `IN`:

```sql
SELECT first_name, last_name
FROM customer
WHERE customer_id IN (SELECT customer_id FROM payment WHERE amount > 10);
```

#### **Explicación:**
1. **Subconsulta**:
    - `SELECT customer_id FROM payment WHERE amount > 10`:
        - Devuelve los `customer_id` de los pagos mayores a $10.

2. **Consulta contenedora**:
    - `SELECT first_name, last_name FROM customer WHERE customer_id IN (...)`:
        - Filtra los clientes cuyos `customer_id` están en la lista devuelta por la subconsulta.

---

