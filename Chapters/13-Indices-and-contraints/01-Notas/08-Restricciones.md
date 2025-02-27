
---

## **Restricciones**

Una restricción es simplemente una limitación colocada en una o más columnas de una tabla. Existen varios tipos diferentes de restricciones, incluyendo:

**Restricciones de clave primaria**
Identifican la columna o columnas que garantizan la unicidad dentro de una tabla.

**Restricciones de clave foránea**
Restringen una o más columnas para contener solo valores que se encuentran en las columnas de clave primaria de otra tabla (también pueden restringir los valores permitidos en otras tablas si se establecen reglas de actualización en cascada o eliminación en cascada).

**Restricciones únicas**
Restringen una o más columnas para contener valores únicos dentro de una tabla (las restricciones de clave primaria son un tipo especial de restricción única).

**Restricciones de comprobación**
Restringen los valores permitidos para una columna.

Sin restricciones, la consistencia de una base de datos es dudosa. Por ejemplo, si el servidor te permite cambiar el ID de un cliente en la tabla **customer** sin cambiar el mismo ID de cliente en la tabla **rental**, terminarás con datos de alquiler que ya no apuntan a registros de clientes válidos (conocidos como filas huérfanas). Sin embargo, con restricciones de clave primaria y clave foránea en su lugar, el servidor ya sea generará un error si se intenta modificar o eliminar datos que son referenciados por otras tablas, o propagará los cambios a otras tablas por ti (más sobre esto en breve).

Si deseas usar restricciones de clave foránea con el servidor MySQL, debes usar el motor de almacenamiento InnoDB para tus tablas.

---

