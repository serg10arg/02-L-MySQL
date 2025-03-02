
---

### **¿Qué es una Expresión `CASE` Simple?**

Una expresión `CASE` simple es una estructura condicional en SQL que permite comparar un valor con una lista de valores posibles y devolver un resultado basado en la coincidencia. Es menos flexible que una expresión `CASE` buscada, ya que solo permite comparaciones de igualdad.

#### Sintaxis:
```sql
CASE valor
    WHEN valor1 THEN resultado1
    WHEN valor2 THEN resultado2
    ...
    WHEN valorN THEN resultadoN
    [ELSE resultado_por_defecto]
END
```

- **`valor`:** Es el valor que se va a comparar.
- **`WHEN valorX THEN resultadoX`:** Si `valor` es igual a `valorX`, devuelve `resultadoX`.
- **`ELSE`:** Especifica el valor a devolver si no hay coincidencias (opcional).
- **`END`:** Cierra la expresión `CASE`.

---

### **Ejemplo de una Expresión `CASE` Simple**

El texto proporciona un ejemplo en el que se clasifican películas según su categoría:

```sql
CASE category.name
    WHEN 'Children' THEN 'All Ages'
    WHEN 'Family' THEN 'All Ages'
    WHEN 'Sports' THEN 'All Ages'
    WHEN 'Animation' THEN 'All Ages'
    WHEN 'Horror' THEN 'Adult'
    WHEN 'Music' THEN 'Teens'
    WHEN 'Games' THEN 'Teens'
    ELSE 'Other'
END
```

#### Explicación:
1. **Comparación de `category.name`:**
    - Si `category.name` es `'Children'`, `'Family'`, `'Sports'` o `'Animation'`, devuelve `'All Ages'`.
    - Si `category.name` es `'Horror'`, devuelve `'Adult'`.
    - Si `category.name` es `'Music'` o `'Games'`, devuelve `'Teens'`.
2. **Cláusula `ELSE`:**
    - Si `category.name` no coincide con ninguno de los valores anteriores, devuelve `'Other'`.

---

### **Características de las Expresiones `CASE` Simples**

1. **Comparaciones de Igualdad:**
    - Solo permite comparar el valor con una lista de valores posibles usando `=`.
    - No admite condiciones complejas como rangos, desigualdades o combinaciones con `AND`/`OR`.

2. **Evaluación en Orden:**
    - Las comparaciones se evalúan en el orden en que están escritas.
    - Tan pronto como se encuentra una coincidencia, se devuelve el resultado correspondiente y se ignoran las comparaciones restantes.

3. **Tipo de Datos:**
    - Todas las expresiones devueltas por las cláusulas `THEN` deben ser del mismo tipo (por ejemplo, todas cadenas, todas números, etc.).

4. **Cláusula `ELSE`:**
    - Es opcional, pero se recomienda incluirla para manejar casos no cubiertos por las condiciones `WHEN`.

---

### **Comparación entre `CASE` Simple y `CASE` Buscada**

| Característica               | `CASE` Simple                          | `CASE` Buscada                          |
|-------------------------------|----------------------------------------|-----------------------------------------|
| **Flexibilidad**              | Solo comparaciones de igualdad (`=`).  | Admite condiciones complejas (rangos, desigualdades, `AND`/`OR`). |
| **Sintaxis**                  | `CASE valor WHEN ... THEN ... END`.    | `CASE WHEN condición THEN ... END`.     |
| **Uso Recomendado**           | Para lógica simple y directa.          | Para lógica condicional compleja.       |
| **Ejemplo**                   | `CASE category.name WHEN 'Horror' THEN 'Adult'`. | `CASE WHEN category.name = 'Horror' THEN 'Adult'`. |

---

### **¿Cuándo Usar `CASE` Simple?**

Las expresiones `CASE` simples son útiles en situaciones donde:
- Solo necesitas comparar un valor con una lista de valores posibles.
- La lógica condicional es simple y no requiere condiciones complejas.

#### Ejemplo de Uso:
```sql
SELECT title,
       CASE rating
           WHEN 'G' THEN 'General Audiences'
           WHEN 'PG' THEN 'Parental Guidance'
           WHEN 'PG-13' THEN 'Parents Strongly Cautioned'
           WHEN 'R' THEN 'Restricted'
           ELSE 'Not Rated'
       END AS rating_description
FROM film;
```

#### Resultado:
| title           | rating_description         |
|-----------------|---------------------------|
| ACADEMY DINOSAUR| General Audiences         |
| ACE GOLDFINGER  | Parental Guidance         |
| ADAPTATION HOLES| Parents Strongly Cautioned|
| AFFAIR PREJUDICE| Restricted               |
| ...             | ...                       |

---

### **Conclusión**

- **Expresiones `CASE` Simples:** Son ideales para comparaciones directas y lógica condicional simple.
- **Limitaciones:** No admiten condiciones complejas como rangos o desigualdades.
- **Recomendación:** Usa expresiones `CASE` buscadas para lógica condicional más compleja.

En resumen, las expresiones `CASE` simples son una herramienta útil para manejar comparaciones directas en SQL, pero para lógica más compleja, es mejor optar por expresiones `CASE` buscadas.