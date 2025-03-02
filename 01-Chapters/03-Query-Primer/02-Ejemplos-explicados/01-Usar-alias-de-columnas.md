
### Desglose de la Sentencia SQL

```sql
SELECT language_id,
       'COMMON' language_usage,
       language_id * 3.1415927 lang_pi_value,
       UPPER(name) language_name
FROM language;
```

#### Partes de la Sentencia

1. **`SELECT`**:
  - Esta cláusula define qué columnas y datos deseas recuperar de la base de datos.

2. **`language_id`**:
  - Esta columna selecciona los valores de `language_id` tal como están en la tabla `language`.

3. **`'COMMON' language_usage`**:
  - Esta expresión crea una columna constante llamada `language_usage` y asigna el valor `'COMMON'` a cada fila del conjunto de resultados. Es decir, cada fila en el resultado tendrá la palabra `'COMMON'` en esta columna.

4. **`language_id * 3.1415927 lang_pi_value`**:
  - Esta expresión toma el valor de `language_id` y lo multiplica por `3.1415927` (el valor aproximado de π) y asigna el resultado a una nueva columna llamada `lang_pi_value`.

5. **`UPPER(name) language_name`**:
  - Esta expresión toma el valor de la columna `name` y lo convierte a mayúsculas utilizando la función `UPPER()`, y asigna el resultado a una nueva columna llamada `language_name`.

6. **`FROM language`**:
  - Esta cláusula especifica la tabla `language` de la cual se recuperarán los datos.

### Ejemplo de Resultados

Supongamos que la tabla `language` contiene los siguientes datos:

| language_id | name     |
|-------------|----------|
| 1           | English  |
| 2           | Italian  |
| 3           | Japanese |
| 4           | Mandarin |
| 5           | French   |
| 6           | German   |

La consulta SQL proporcionada devolverá el siguiente conjunto de resultados:

| language_id | language_usage | lang_pi_value | language_name |
|-------------|----------------|---------------|---------------|
| 1           | COMMON         | 3.1415927     | ENGLISH       |
| 2           | COMMON         | 6.2831854     | ITALIAN       |
| 3           | COMMON         | 9.4247781     | JAPANESE      |
| 4           | COMMON         | 12.5663708    | MANDARIN      |
| 5           | COMMON         | 15.7079635    | FRENCH        |
| 6           | COMMON         | 18.8495562    | GERMAN        |

### Explicación del Resultado

- **`language_id`**: Muestra el identificador único del lenguaje.
- **`language_usage`**: Asigna el valor constante `'COMMON'` a cada fila.
- **`lang_pi_value`**: Muestra el resultado de multiplicar `language_id` por `3.1415927`.
- **`language_name`**: Muestra el nombre del lenguaje en mayúsculas.

---
### Uso de la Palabra Clave `AS`

Para que tus alias de columna destaquen aún más, tienes la opción de utilizar la palabra clave `AS` antes del nombre del alias. Esto puede mejorar la legibilidad de la consulta:

```sql
mysql> SELECT language_id,
    ->   'COMMON' AS language_usage,
    ->   language_id * 3.1415927 AS lang_pi_value,
    ->   UPPER(name) AS language_name
    -> FROM language;
```

### Beneficios de Utilizar Alias de Columna

1. **Claridad**: Los alias hacen que el resultado sea más comprensible.
2. **Compatibilidad**: Facilitan el trabajo programático si emites la consulta desde lenguajes de programación como Java o Python.
3. **Legibilidad**: Usar `AS` puede mejorar la legibilidad de tus consultas.

Usar alias de columna es una práctica recomendada que puede hacer que tus consultas sean más fáciles de entender y mantener. 