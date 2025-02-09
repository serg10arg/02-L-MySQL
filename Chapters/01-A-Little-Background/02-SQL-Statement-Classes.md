### Clases de Sentencias SQL

**1. ¿Qué es SQL?**
- SQL (Structured Query Language) es un lenguaje de programación utilizado para gestionar y manipular bases de datos relacionales.
- Se divide en varias categorías: sentencias de esquema, sentencias de datos y sentencias de transacción.

**2. Sentencias de Esquema SQL**
- **Definición**: Las sentencias de esquema se utilizan para definir las estructuras de datos que se almacenarán en la base de datos.
- **Ejemplo**: `CREATE TABLE` se usa para crear nuevas tablas.
   - **Análisis**: Una tabla es una estructura que contiene datos organizados en filas y columnas.
   - **Ejemplo de Sintaxis**:
     ```sql
     CREATE TABLE corporation
     (corp_id SMALLINT,
      name VARCHAR(30),
      CONSTRAINT pk_corporation PRIMARY KEY (corp_id));
     ```

**3. Sentencias de Datos SQL**
- **Definición**: Las sentencias de datos se utilizan para manipular las estructuras de datos previamente definidas.
- **Ejemplo**: `INSERT INTO` se usa para añadir datos a una tabla.
   - **Análisis**: Añadir datos significa insertar nuevas filas en una tabla existente.
   - **Ejemplo de Sintaxis**:
     ```sql
     INSERT INTO corporation (corp_id, name)
     VALUES (27, 'Acme Paper Corporation');
     ```

**4. Sentencias de Transacción SQL**
- **Definición**: Las sentencias de transacción se utilizan para gestionar transacciones en la base de datos, como comenzar, terminar y deshacer transacciones.
- **Concepto**: Una transacción es una unidad de trabajo que se ejecuta de manera completa o no se ejecuta en absoluto, garantizando la integridad de los datos.

**5. Diccionario de Datos y Metadatos**
- **Definición**: El diccionario de datos es un conjunto especial de tablas que almacena metadatos, que son datos sobre la base de datos misma.
- **Utilidad**: Los metadatos permiten descubrir y gestionar las estructuras de datos actuales en la base de datos en tiempo de ejecución.

### Aplicación de Sentencias SQL

**1. Crear Tablas**
- Usar sentencias de esquema (`CREATE TABLE`) para definir las estructuras de datos.

**2. Manipular Datos**
- Usar sentencias de datos (`INSERT`, `SELECT`, `UPDATE`, `DELETE`) para añadir, consultar, modificar y eliminar datos.

**3. Gestionar Transacciones**
- Usar sentencias de transacción (`BEGIN`, `COMMIT`, `ROLLBACK`) para garantizar la integridad de las operaciones de datos.

