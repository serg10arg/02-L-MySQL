
### La Sentencia SQL:
```sql
SELECT CEIL(72.000000001), FLOOR(72.999999999);
```

### ¿Qué hace esta sentencia?

1. **SELECT**: Es una palabra clave en SQL que se utiliza para seleccionar datos. En este caso, estamos seleccionando el resultado de dos funciones: `CEIL` y `FLOOR`.

2. **CEIL**: Es una función en SQL que redondea un número **hacia arriba** al entero más cercano. Si el número tiene una parte decimal, no importa cuán pequeña sea, `CEIL` lo redondeará al siguiente número entero.

3. **FLOOR**: Es una función en SQL que redondea un número **hacia abajo** al entero más cercano. Si el número tiene una parte decimal, no importa cuán cercano esté al siguiente entero, `FLOOR` lo redondeará al número entero anterior.

---

### Desglosando los argumentos:

#### 1. `CEIL(72.000000001)`:
- El número proporcionado es `72.000000001`.
- Aunque la parte decimal es extremadamente pequeña (`0.000000001`), `CEIL` redondea **hacia arriba** al siguiente número entero.
- Por lo tanto, `CEIL(72.000000001)` devuelve `73`.

#### 2. `FLOOR(72.999999999)`:
- El número proporcionado es `72.999999999`.
- Aunque la parte decimal está muy cerca de `1` (`0.999999999`), `FLOOR` redondea **hacia abajo** al número entero anterior.
- Por lo tanto, `FLOOR(72.999999999)` devuelve `72`.

---

### Resultado:
```sql
+--------------------+---------------------+
| CEIL(72.000000001) | FLOOR(72.999999999) |
+--------------------+---------------------+
|                 73 |                  72 |
+--------------------+---------------------+
```

---

### Resumen:
- **`CEIL`**: Redondea un número **hacia arriba** al siguiente entero.
    - Ejemplo: `CEIL(72.000000001)` = `73`.
- **`FLOOR`**: Redondea un número **hacia abajo** al entero anterior.
    - Ejemplo: `FLOOR(72.999999999)` = `72`.

Estas funciones son útiles cuando necesitas redondear números en SQL, ya sea para cálculos matemáticos, informes o análisis de datos.

