
---

### **Puntos de Guardado en Transacciones**

Los **puntos de guardado** (savepoints) son una característica que te permite establecer marcadores dentro de una transacción. Estos marcadores te permiten revertir a un punto específico dentro de la transacción, en lugar de deshacer toda la transacción.

#### ¿Por qué usar Puntos de Guardado?
- **Flexibilidad:** Si encuentras un problema en una parte de la transacción, puedes revertir solo hasta un punto de guardado específico, en lugar de deshacer todo el trabajo realizado.
- **Control:** Te permite manejar errores de manera más granular, manteniendo los cambios válidos realizados antes del punto de guardado.

---

### **Crear y Usar Puntos de Guardado**

1. **Crear un Punto de Guardado:**
    - Usa el comando `SAVEPOINT` seguido de un nombre único.
    - **Ejemplo:**
      ```sql
      SAVEPOINT my_savepoint;
      ```

2. **Revertir a un Punto de Guardado:**
    - Usa el comando `ROLLBACK TO SAVEPOINT` seguido del nombre del punto de guardado.
    - **Ejemplo:**
      ```sql
      ROLLBACK TO SAVEPOINT my_savepoint;
      ```

3. **Confirmar la Transacción:**
    - Si todo está bien, usa `COMMIT` para confirmar los cambios.
    - **Ejemplo:**
      ```sql
      COMMIT;
      ```

---

### **Ejemplo de Uso de Puntos de Guardado**

El texto proporciona un ejemplo en el que se retira un producto (`XYZ`) pero no se cierran las cuentas asociadas.

#### Consulta:
```sql
START TRANSACTION;

-- Retirar el producto XYZ
UPDATE product
SET date_retired = CURRENT_TIMESTAMP()
WHERE product_cd = 'XYZ';

-- Crear un punto de guardado
SAVEPOINT before_close_accounts;

-- Intentar cerrar las cuentas asociadas al producto XYZ
UPDATE account
SET status = 'CLOSED', close_date = CURRENT_TIMESTAMP(),
 last_activity_date = CURRENT_TIMESTAMP()
WHERE product_cd = 'XYZ';

-- Revertir al punto de guardado (no cerrar las cuentas)
ROLLBACK TO SAVEPOINT before_close_accounts;

-- Confirmar la transacción
COMMIT;
```

#### Explicación:
1. **Retirar el Producto:**
    - Se actualiza la columna `date_retired` del producto `XYZ`.

2. **Punto de Guardado:**
    - Se crea un punto de guardado llamado `before_close_accounts`.

3. **Cerrar Cuentas:**
    - Se intenta cerrar las cuentas asociadas al producto `XYZ`.

4. **Revertir al Punto de Guardado:**
    - Si algo sale mal al cerrar las cuentas, se revierte solo esa parte de la transacción.

5. **Confirmar la Transacción:**
    - Se confirman los cambios realizados antes del punto de guardado (retirar el producto).

---

### **Consideraciones sobre Puntos de Guardado**

1. **No se Guarda Nada Automáticamente:**
    - Los puntos de guardado no confirman los cambios. Debes usar `COMMIT` para hacerlos permanentes.

2. **Revertir sin Punto de Guardado:**
    - Si usas `ROLLBACK` sin especificar un punto de guardado, se deshace toda la transacción.

3. **Compatibilidad:**
    - En **SQL Server**, debes usar los comandos propietarios:
        - Crear un punto de guardado: `SAVE TRANSACTION my_savepoint;`
        - Revertir a un punto de guardado: `ROLLBACK TRANSACTION my_savepoint;`

---

### **Elección de un Motor de Almacenamiento**

MySQL es único en cuanto a que permite elegir diferentes motores de almacenamiento para cada tabla. Estos motores determinan cómo se almacenan y gestionan los datos, incluyendo el manejo de transacciones y bloqueos.

#### Motores de Almacenamiento en MySQL:
1. **MyISAM:**
    - No transaccional.
    - Usa bloqueo de tabla.
    - Ideal para aplicaciones de solo lectura o con pocas escrituras.

2. **MEMORY:**
    - No transaccional.
    - Almacena datos en memoria.
    - Ideal para tablas temporales o cachés.

3. **CSV:**
    - Transaccional.
    - Almacena datos en archivos CSV.
    - Útil para importar/exportar datos.

4. **InnoDB:**
    - Transaccional.
    - Usa bloqueo a nivel de fila.
    - Proporciona alta concurrencia.
    - Recomendado para tablas que participan en transacciones.

5. **Merge:**
    - Especializado en unir múltiples tablas MyISAM.
    - Útil para particionamiento de tablas.

6. **Archive:**
    - Especializado en almacenar grandes cantidades de datos sin indexar.
    - Ideal para propósitos de archivo.

---

### **Cambiar el Motor de Almacenamiento**

Puedes especificar el motor de almacenamiento al crear una tabla o cambiar el motor de una tabla existente.

#### Ver el Motor de una Tabla:
```sql
SHOW TABLE STATUS LIKE 'customer' \G;
```

#### Cambiar el Motor de una Tabla:
```sql
ALTER TABLE customer ENGINE = INNODB;
```

---

### **Conclusión**

- **Puntos de Guardado:** Son útiles para revertir solo una parte de una transacción, manteniendo los cambios válidos.
- **Motores de Almacenamiento:** MySQL ofrece múltiples motores, pero **InnoDB** es el recomendado para tablas transaccionales debido a su soporte de bloqueo a nivel de fila y alta concurrencia.
- **Flexibilidad:** MySQL permite elegir el motor de almacenamiento por tabla, lo que te da control sobre cómo se gestionan los datos.

En resumen, los puntos de guardado y la elección del motor de almacenamiento son herramientas poderosas que te permiten manejar transacciones y optimizar el rendimiento de tu base de datos. 