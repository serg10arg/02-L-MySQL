
### La Sentencia SQL:
```sql
SELECT ROUND(17, -1), TRUNCATE(17, -1);
```

### ¿Qué hace esta sentencia?

1. **SELECT**: Es una palabra clave en SQL que se utiliza para seleccionar datos. En este caso, estamos seleccionando el resultado de dos funciones: `ROUND` y `TRUNCATE`, aplicadas al número `17` con un segundo argumento negativo (`-1`).

2. **ROUND**: Es una función en SQL que redondea un número a un número específico de decimales. Cuando el segundo argumento es negativo, redondea el número a la izquierda del punto decimal (es decir, a la decena, centena, etc., más cercana).

3. **TRUNCATE**: Es una función en SQL que trunca un número a un número específico de decimales. Cuando el segundo argumento es negativo, trunca el número a la izquierda del punto decimal (es decir, elimina dígitos y los reemplaza con ceros).

---

### Desglosando los argumentos:

#### 1. `ROUND(17, -1)`:
- El número proporcionado es `17`.
- El segundo argumento es `-1`, lo que significa que queremos redondear el número a la **decena más cercana**.
- Para redondear a la decena más cercana, miramos el dígito en la posición de las unidades (en este caso, `7`).
    - Si el dígito de las unidades es **5 o mayor**, redondeamos hacia arriba.
    - Si es **menor que 5**, redondeamos hacia abajo.
- En este caso, `7` es mayor que `5`, por lo que redondeamos hacia arriba.
- Por lo tanto, `ROUND(17, -1)` devuelve `20`.

#### 2. `TRUNCATE(17, -1)`:
- El número proporcionado es `17`.
- El segundo argumento es `-1`, lo que significa que queremos truncar el número a la **decena más cercana**.
- Para truncar a la decena más cercana, simplemente reemplazamos el dígito de las unidades con `0`.
- Por lo tanto, `TRUNCATE(17, -1)` devuelve `10`.

---

### Resultado:
```sql
+---------------+------------------+
| ROUND(17, -1) | TRUNCATE(17, -1) |
+---------------+------------------+
|            20 |               10 |
+---------------+------------------+
```

---

### Resumen:
- **`ROUND(17, -1)`**: Redondea el número `17` a la **decena más cercana**.
    - Como el dígito de las unidades (`7`) es mayor que `5`, redondea hacia arriba a `20`.
- **`TRUNCATE(17, -1)`**: Trunca el número `17` a la **decena más cercana**.
    - Simplemente reemplaza el dígito de las unidades con `0`, resultando en `10`.

---

### Diferencia entre `ROUND` y `TRUNCATE` con argumentos negativos:
- **`ROUND`**: Redondea el número al múltiplo de 10, 100, etc., más cercano, dependiendo del valor del segundo argumento negativo.
- **`TRUNCATE`**: Simplemente reemplaza los dígitos a la derecha del punto especificado con ceros, **sin redondear**.

Por ejemplo:
- `ROUND(17, -1)` = `20` (redondea a la decena más cercana).
- `TRUNCATE(17, -1)` = `10` (trunca a la decena más cercana sin redondear).

---

