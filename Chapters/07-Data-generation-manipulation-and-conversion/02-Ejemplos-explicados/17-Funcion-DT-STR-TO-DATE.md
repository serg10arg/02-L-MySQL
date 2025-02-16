
### La Sentencia SQL:
```sql
UPDATE rental
SET return_date = STR_TO_DATE('September 17, 2019', '%M %d, %Y')
WHERE rental_id = 99999;
```

### ¿Qué hace esta sentencia?

1. **UPDATE**: Es una palabra clave en SQL que se utiliza para modificar datos existentes en una tabla. En este caso, estamos actualizando la tabla `rental`.

2. **SET**: Especifica la columna que se va a actualizar y el nuevo valor que se le asignará. En este caso, estamos actualizando la columna `return_date`.

3. **STR_TO_DATE**: Es una función en SQL que convierte una cadena de texto en un valor de tipo `DATE`. Toma dos argumentos:
    - **Primer argumento**: La cadena de texto que se va a convertir. En este caso, es `'September 17, 2019'`.
    - **Segundo argumento**: El formato de la cadena de texto. En este caso, es `'%M %d, %Y'`, que indica:
        - `%M`: Nombre completo del mes (en inglés).
        - `%d`: Día del mes (con dos dígitos).
        - `%Y`: Año con cuatro dígitos.

4. **WHERE**: Especifica la condición que deben cumplir las filas para ser actualizadas. En este caso, estamos actualizando la fila donde `rental_id = 99999`.

---

### Desglosando los argumentos:

#### 1. `STR_TO_DATE('September 17, 2019', '%M %d, %Y')`:
- La cadena `'September 17, 2019'` representa una fecha en formato de texto.
- El formato `'%M %d, %Y'` indica cómo se debe interpretar la cadena:
    - `%M`: El mes es `September`.
    - `%d`: El día es `17`.
    - `%Y`: El año es `2019`.
- La función `STR_TO_DATE` convierte esta cadena en un valor de tipo `DATE`, que es `2019-09-17`.

#### 2. `WHERE rental_id = 99999`:
- Esta condición asegura que solo se actualice la fila donde el `rental_id` es `99999`.
- Si no se incluye la cláusula `WHERE`, todas las filas de la tabla `rental` se actualizarían, lo cual no es deseable en este caso.

---

### Resultado:
- La fila de la tabla `rental` con `rental_id = 99999` tendrá su columna `return_date` actualizada a `2019-09-17`.

---

### Resumen:
- **`UPDATE`**: Modifica datos existentes en una tabla.
- **`SET`**: Especifica la columna y el nuevo valor.
- **`STR_TO_DATE`**: Convierte una cadena de texto en un valor de tipo `DATE`.
- **`WHERE`**: Filtra las filas que se actualizarán.

---

### Ejemplo de la tabla `rental` (antes y después):

#### Antes de la actualización:
| rental_id | return_date  |
|-----------|--------------|
| 99999     | NULL         |
| 12345     | 2019-08-15   |

#### Después de la actualización:
| rental_id | return_date  |
|-----------|--------------|
| 99999     | 2019-09-17   |
| 12345     | 2019-08-15   |

---

### ¿Para qué sirve esto?
- **`STR_TO_DATE`**: Es útil cuando tienes fechas en formato de texto y necesitas convertirlas a un tipo de dato `DATE` para realizar operaciones de fecha.
- **`UPDATE`**: Es útil para modificar datos existentes en una tabla, como corregir fechas, actualizar estados, etc.

---

