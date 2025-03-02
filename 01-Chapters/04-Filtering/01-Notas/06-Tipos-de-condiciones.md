### Tipos de Condiciones

Existen diferentes formas de filtrar datos no deseados en SQL. Puedes buscar valores específicos, conjuntos de valores o rangos de valores para incluir o excluir, o puedes utilizar técnicas de búsqueda de patrones para buscar coincidencias parciales al tratar con datos de cadenas.

### 1. Condiciones de Igualdad

Un gran porcentaje de las condiciones de filtro que escribirás o encontraras tendrán la forma 'columna = expresión', como en:

- `title = 'RIVER OUTLAW'`
- `fed_id = '111-11-1111'`
- `amount = 375.25`
- `film_id = (SELECT film_id FROM film WHERE title = 'RIVER OUTLAW')`

Estas condiciones se denominan condiciones de igualdad porque equiparan una expresión con otra. Los tres primeros ejemplos equiparan una columna a un valor literal (dos cadenas y un número), y el cuarto ejemplo equipara una columna al valor devuelto por una subconsulta.

Ejemplo:

```sql
mysql> SELECT c.email
    -> FROM customer c
    ->   INNER JOIN rental r
    ->   ON c.customer_id = r.customer_id
    -> WHERE date(r.rental_date) = '2005-06-14';
```

### 2. Condiciones de Desigualdad

Las condiciones de desigualdad aseguran que dos expresiones no sean iguales. Aquí está la consulta anterior con la condición de filtro en la cláusula `WHERE` cambiada a una condición de desigualdad:

```sql
mysql> SELECT c.email
    -> FROM customer c
    ->   INNER JOIN rental r
    ->   ON c.customer_id = r.customer_id
    -> WHERE date(r.rental_date) <> '2005-06-14';
```

Las condiciones de desigualdad pueden usar los operadores `!=` o `<>`.

### Modificación de Datos Utilizando Condiciones de Igualdad

Las condiciones de igualdad/desigualdad son comúnmente utilizadas al modificar datos. Por ejemplo, para eliminar filas de la tabla `rental` donde la fecha de alquiler fue en 2004, puedes usar:

```sql
DELETE FROM rental
WHERE year(rental_date) = 2004;
```

O para eliminar filas donde la fecha de alquiler no fue en 2005 o 2006:

```sql
DELETE FROM rental
WHERE year(rental_date) <> 2005 AND year(rental_date) <> 2006;
```

### Conclusión

Las condiciones de igualdad y desigualdad son esenciales para filtrar y manipular datos en SQL. Permiten identificar, excluir o modificar datos específicos de manera eficiente. Asegúrate de comprender cómo y cuándo utilizar estos tipos de condiciones para optimizar tus consultas y operaciones de datos.