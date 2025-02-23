
---

### **¿Qué es una Expresión `CASE` Buscada?**

Una expresión `CASE` buscada es una estructura condicional en SQL que permite evaluar múltiples condiciones y devolver un valor dependiendo de cuál condición sea verdadera. A diferencia de una expresión `CASE` simple (que compara una sola expresión con múltiples valores), una expresión `CASE` buscada evalúa condiciones más complejas.

#### Sintaxis:
```sql
CASE
    WHEN condición1 THEN valor1
    WHEN condición2 THEN valor2
    ...
    WHEN condiciónN THEN valorN
    [ELSE valor_por_defecto]
END
```

- **`WHEN`:** Evalúa una condición. Si es verdadera, devuelve el valor asociado (`THEN`).
- **`THEN`:** Especifica el valor a devolver si la condición es verdadera.
- **`ELSE`:** Especifica el valor a devolver si ninguna de las condiciones es verdadera (opcional).
- **`END`:** Cierra la expresión `CASE`.

---

### **Ejemplo de una Expresión `CASE` Buscada**

El texto proporciona un ejemplo en el que se clasifican películas según su categoría:

```sql
CASE
    WHEN category.name IN ('Children', 'Family', 'Sports', 'Animation') THEN 'All Ages'
    WHEN category.name = 'Horror' THEN 'Adult'
    WHEN category.name IN ('Music', 'Games') THEN 'Teens'
    ELSE 'Other'
END
```

#### Explicación:
1. **Primera Condición (`WHEN`):**
    - Si `category.name` es `'Children'`, `'Family'`, `'Sports'` o `'Animation'`, devuelve `'All Ages'`.
2. **Segunda Condición (`WHEN`):**
    - Si `category.name` es `'Horror'`, devuelve `'Adult'`.
3. **Tercera Condición (`WHEN`):**
    - Si `category.name` es `'Music'` o `'Games'`, devuelve `'Teens'`.
4. **Cláusula `ELSE`:**
    - Si ninguna de las condiciones anteriores es verdadera, devuelve `'Other'`.

---

### **Características de las Expresiones `CASE` Buscadas**

1. **Evaluación en Orden:**
    - Las condiciones se evalúan en el orden en que están escritas.
    - Tan pronto como una condición es verdadera, se devuelve el valor correspondiente y se ignoran las condiciones restantes.

2. **Tipo de Datos:**
    - Todas las expresiones devueltas por las cláusulas `THEN` deben ser del mismo tipo (por ejemplo, todas cadenas, todas números, etc.).

3. **Cláusula `ELSE`:**
    - Es opcional, pero se recomienda incluirla para manejar casos no cubiertos por las condiciones `WHEN`.

---

### **Uso de Subconsultas en Expresiones `CASE`**

Las expresiones `CASE` pueden devolver cualquier tipo de expresión, incluidas **subconsultas**. Esto permite realizar cálculos complejos o recuperar datos adicionales dependiendo de las condiciones.

#### Ejemplo con Subconsulta:
```sql
SELECT c.first_name, c.last_name,
       CASE
           WHEN active = 0 THEN 0
           ELSE (SELECT COUNT(*) FROM rental r
                 WHERE r.customer_id = c.customer_id)
       END AS num_rentals
FROM customer c;
```

#### Resultado:
| first_name | last_name | num_rentals |
|------------|-----------|-------------|
| MARY       | SMITH     |          32 |
| PATRICIA   | JOHNSON   |          27 |
| LINDA      | WILLIAMS  |          26 |
| BARBARA    | JONES     |          22 |
| ELIZABETH  | BROWN     |          38 |
| JENNIFER   | DAVIS     |          28 |
| TERRANCE   | ROUSH     |           0 |
| RENE       | MCALISTER |          26 |
| EDUARDO    | HIATT     |          27 |
| TERRENCE   | GUNDERSON |          30 |
| ENRIQUE    | FORSYTHE  |          28 |
| FREDDIE    | DUGGAN    |          25 |
| WADE       | DELVALLE  |          22 |
| AUSTIN     | CINTRON   |          19 |

#### Explicación:
1. **Primera Condición (`WHEN`):**
    - Si `active = 0`, devuelve `0` (el cliente está inactivo y no tiene alquileres).
2. **Cláusula `ELSE`:**
    - Si el cliente está activo (`active = 1`), ejecuta una subconsulta para contar el número de alquileres (`rental`) asociados a ese cliente.
3. **Subconsulta Correlacionada:**
    - La subconsulta `(SELECT COUNT(*) FROM rental r WHERE r.customer_id = c.customer_id)` cuenta los alquileres para cada cliente activo.

---

### **Ventajas de Usar Expresiones `CASE` Buscadas**

1. **Flexibilidad:**
    - Permite manejar múltiples condiciones complejas en una sola expresión.
    - Puede devolver diferentes tipos de valores (números, cadenas, fechas, etc.).

2. **Eficiencia:**
    - En algunos casos, puede ser más eficiente que unir tablas o realizar múltiples consultas.
    - Ejemplo: En lugar de unir las tablas `customer` y `rental`, se usa una subconsulta correlacionada para contar los alquileres solo para clientes activos.

3. **Legibilidad:**
    - Mejora la claridad del código al encapsular la lógica condicional en una sola expresión.

---

### **Conclusión**

- **Expresiones `CASE` Buscadas:** Son una herramienta poderosa para implementar lógica condicional en SQL.
- **Usos Comunes:** Clasificación de datos, traducción de valores, cálculos condicionales y subconsultas.
- **Ventajas:** Flexibilidad, eficiencia y mejora de la legibilidad del código.

En resumen, las expresiones `CASE` buscadas son esenciales para manejar lógica condicional compleja en SQL, permitiéndote tomar decisiones basadas en múltiples condiciones y mejorar la claridad de tus consultas.