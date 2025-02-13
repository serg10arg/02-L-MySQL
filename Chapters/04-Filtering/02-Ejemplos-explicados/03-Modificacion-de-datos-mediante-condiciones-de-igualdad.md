
### **Sentencia SQL**

```sql
DELETE FROM rental
WHERE year(rental_date) <> 2005 AND year(rental_date) <> 2006;
```

### **Explicación Paso a Paso**

---

#### 1. **`DELETE FROM`**
```sql
DELETE FROM rental
```
- **`DELETE FROM`**: Indica que queremos eliminar filas de la tabla especificada.
- **`rental`**: Especificamos que estamos trabajando con la tabla `rental`, que contiene información sobre los alquileres.

---

#### 2. **`WHERE`**
```sql
WHERE year(rental_date) <> 2005 AND year(rental_date) <> 2006
```
- **`year(rental_date)`**: La función `year()` extrae el año de la columna `rental_date` (que probablemente contiene una fecha y hora completa).
- **`<> 2005` y `<> 2006`**: El operador `<>` significa "no igual a". Aquí estamos filtrando las filas donde el año de `rental_date` no es `2005` **y** no es `2006`.
- **`AND`**: Ambas condiciones deben ser verdaderas al mismo tiempo. Es decir, el año de `rental_date` no debe ser `2005` **ni** `2006`.

---

### **¿Qué hace esta sentencia en conjunto?**

1. **Elimina filas de la tabla `rental`**: Se eliminarán todas las filas donde el año de `rental_date` no sea `2005` ni `2006`.
2. **Filtra por año**: Solo se eliminarán los alquileres que no ocurrieron en los años `2005` o `2006`.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `rental`:

| rental_id | rental_date        |
|-----------|--------------------|
| 1         | 2005-06-14 10:30:00|
| 2         | 2006-07-15 15:45:00|
| 3         | 2004-08-16 09:00:00|
| 4         | 2007-09-17 12:00:00|
| 5         | 2005-10-18 18:30:00|

---

#### Resultado de la Sentencia
Después de ejecutar la sentencia `DELETE`, la tabla `rental` quedará así:

| rental_id | rental_date        |
|-----------|--------------------|
| 1         | 2005-06-14 10:30:00|
| 2         | 2006-07-15 15:45:00|
| 5         | 2005-10-18 18:30:00|

- **Explicación**:
    - Los alquileres con `rental_id = 3` y `4` ocurrieron en los años `2004` y `2007`, respectivamente. Como estos años no son `2005` ni `2006`, estas filas serán eliminadas.
    - Los alquileres con `rental_id = 1`, `2` y `5` ocurrieron en los años `2005` y `2006`, por lo que no serán eliminados.

---

### **Resumen**

- **`DELETE FROM`**: Elimina filas de la tabla `rental`.
- **`WHERE`**: Filtra las filas para eliminar solo aquellas donde el año de `rental_date` no sea `2005` ni `2006`.

