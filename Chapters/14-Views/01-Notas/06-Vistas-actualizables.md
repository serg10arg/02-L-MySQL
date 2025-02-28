
---

## **Vistas Actualizables**

Si proporcionas a los usuarios un conjunto de vistas para la recuperación de datos, ¿qué deberías hacer si los usuarios también necesitan modificar esos mismos datos? Podría parecer un poco extraño, por ejemplo, obligar a los usuarios a recuperar datos mediante una vista pero permitirles modificar directamente la tabla subyacente utilizando declaraciones **UPDATE** o **INSERT**. Para este propósito, MySQL, Oracle Database y SQL Server permiten modificar datos a través de una vista, siempre y cuando se cumplan ciertas restricciones.

En el caso de MySQL, una vista es actualizable si se cumplen las siguientes condiciones:
- No se utilizan funciones de agregación (**MAX()**, **MIN()**, **AVG()**, etc.).
- La vista no emplea cláusulas **GROUP BY** o **HAVING**.
- No existen subconsultas en las cláusulas **SELECT** o **FROM**, y cualquier subconsulta en la cláusula **WHERE** no hace referencia a tablas en la cláusula **FROM**.
- La vista no utiliza **UNION**, **UNION ALL**, o **DISTINCT**.
- La cláusula **FROM** incluye al menos una tabla o vista actualizable.
- La cláusula **FROM** usa únicamente **INNER JOIN** si hay más de una tabla o vista.

---

El siguiente paso para demostrar la utilidad de las vistas actualizables es empezar con una definición de vista simple y, posteriormente, avanzar hacia una vista más compleja.

---

