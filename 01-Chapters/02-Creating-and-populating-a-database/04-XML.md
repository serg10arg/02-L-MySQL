### ¿Puedes Obtener eso en XML?

Si vas a trabajar con datos XML, estarás contento de saber que la mayoría de los servidores de bases de datos proporcionan una forma sencilla de generar salida XML a partir de una consulta. Con MySQL, por ejemplo, puedes usar la opción `--xml` al invocar la herramienta `mysql`, y toda tu salida se formateará automáticamente utilizando XML.

#### Ejemplo en MySQL

1. **Invocar la Herramienta MySQL con la Opción `--xml`**:
   ```sh
   C:\database> mysql -u lrngsql -p --xml bank
   Enter password: xxxxxx
   Welcome to the MySQL Monitor...
   ```

2. **Consulta y Resultado en Formato XML**:
   ```sql
   mysql> SELECT * FROM favorite_food;
   ```
   **Resultado**:
   ```xml
   <?xml version="1.0"?>
   <resultset statement="select * from favorite_food"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
     <row>
           <field name="person_id">1</field>
           <field name="food">cookies</field>
     </row>
     <row>
           <field name="person_id">1</field>
           <field name="food">nachos</field>
     </row>
     <row>
           <field name="person_id">1</field>
           <field name="food">pizza</field>
     </row>
   </resultset>
   ```
   **Mensaje**:
   ```
   3 rows in set (0.00 sec)
   ```

Con esta opción, todos los datos de salida de tus consultas se formatearán automáticamente en XML, lo cual es muy útil cuando necesitas procesar o transferir datos en este formato.

#### Ejemplo en SQL Server

Con SQL Server, no necesitas configurar tu herramienta de línea de comandos; solo necesitas agregar la cláusula `FOR XML` al final de tu consulta:

```sql
SELECT * FROM favorite_food
FOR XML AUTO, ELEMENTS;
```

Esto generará la salida en formato XML directamente en el resultado de la consulta.

