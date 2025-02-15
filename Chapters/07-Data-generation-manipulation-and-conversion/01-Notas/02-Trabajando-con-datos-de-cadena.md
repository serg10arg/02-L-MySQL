
---

# Trabajando con Datos de Cadena

Cuando trabajas con datos de cadena, estarás utilizando uno de los siguientes tipos de datos de caracteres:

### CHAR
Sostiene cadenas de longitud fija, rellenadas con espacios en blanco. MySQL permite valores CHAR de hasta 255 caracteres de longitud, Oracle Database permite hasta 2,000 caracteres y SQL Server permite hasta 8,000 caracteres.

### VARCHAR
Sostiene cadenas de longitud variable. MySQL permite hasta 65,535 caracteres en una columna varchar, Oracle Database (a través del tipo varchar2) permite hasta 4,000 caracteres, y SQL Server permite hasta 8,000 caracteres.

### TEXT (MySQL y SQL Server) o CLOB (Oracle Database)
Sostiene cadenas de longitud variable muy grandes (generalmente referidas como documentos en este contexto). MySQL tiene múltiples tipos de texto (tinytext, text, mediumtext y longtext) para documentos de hasta 4 GB de tamaño. SQL Server tiene un solo tipo de texto para documentos de hasta 2 GB de tamaño, y Oracle Database incluye el tipo de datos CLOB, que puede contener documentos de hasta 128 TB. SQL Server 2005 también incluye el tipo de datos `varchar(max)` y recomienda su uso en lugar del tipo de texto, que se eliminará del servidor en alguna versión futura.

Para demostrar cómo puedes usar estos diversos tipos, utilizo la siguiente tabla para algunos de los ejemplos en esta sección:

```sql
CREATE TABLE string_tbl
(char_fld CHAR(30),
 vchar_fld VARCHAR(30),
 text_fld TEXT
);
```

Las siguientes dos subsecciones muestran cómo puedes generar y manipular datos de cadena.

---

