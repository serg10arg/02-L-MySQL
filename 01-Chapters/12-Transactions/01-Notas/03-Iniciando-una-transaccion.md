
---

### **¿Cómo Inician las Transacciones los Servidores de Bases de Datos?**

Los servidores de bases de datos manejan la creación de transacciones de dos maneras principales:

1. **Transacciones Implícitas:**
    - Una transacción activa siempre está asociada con una sesión de base de datos.
    - No hay necesidad de iniciar explícitamente una transacción.
    - Cuando la transacción actual termina, el servidor automáticamente comienza una nueva transacción para la sesión.
    - **Ejemplo:** Oracle Database.

2. **Modo de Autocommit:**
    - Las declaraciones SQL individuales se confirman automáticamente de manera independiente.
    - Para comenzar una transacción, debes emitir un comando explícito (`START TRANSACTION` o `BEGIN TRANSACTION`).
    - **Ejemplo:** Microsoft SQL Server y MySQL.

---

### **Transacciones Implícitas (Oracle Database)**

En Oracle Database, cada sesión tiene una transacción activa implícita. Esto significa que:
- No necesitas iniciar explícitamente una transacción.
- Puedes revertir los cambios en cualquier momento antes de confirmar la transacción.
- **Ventaja:** Proporciona flexibilidad para deshacer cambios, incluso en declaraciones SQL individuales.

#### Ejemplo:
```sql
UPDATE account SET avail_balance = avail_balance - 500
WHERE account_id = 9988;

/* Si algo sale mal, puedes revertir los cambios */
ROLLBACK;
```

---

### **Modo de Autocommit (MySQL y SQL Server)**

En MySQL y SQL Server, las declaraciones SQL individuales se confirman automáticamente a menos que inicies explícitamente una transacción.

#### Iniciar una Transacción:
- **MySQL:**
  ```sql
  START TRANSACTION;
  ```
- **SQL Server:**
  ```sql
  BEGIN TRANSACTION;
  ```

#### Ejemplo:
```sql
START TRANSACTION;

UPDATE account SET avail_balance = avail_balance - 500
WHERE account_id = 9988;

UPDATE account SET avail_balance = avail_balance + 500
WHERE account_id = 9989;

/* Si todo está bien, confirma los cambios */
COMMIT;

/* Si algo sale mal, revierte los cambios */
ROLLBACK;
```

---

### **Desactivar el Modo de Autocommit**

Puedes desactivar el modo de autocommit para una sesión, lo que hace que el servidor se comporte como Oracle Database (transacciones implícitas).

#### En SQL Server:
```sql
SET IMPLICIT_TRANSACTIONS ON;
```

#### En MySQL:
```sql
SET AUTOCOMMIT=0;
```

#### Ejemplo:
```sql
SET AUTOCOMMIT=0;

UPDATE account SET avail_balance = avail_balance - 500
WHERE account_id = 9988;

/* Los cambios no se confirman automáticamente */
COMMIT;  /* o ROLLBACK; */
```

---

### **Consejo: Desactiva el Modo de Autocommit**

El texto recomienda desactivar el modo de autocommit cada vez que inicies sesión y ejecutar todas las declaraciones SQL dentro de una transacción. Esto tiene varias ventajas:
1. **Control sobre los Cambios:**
    - Puedes confirmar o revertir los cambios según sea necesario.
2. **Prevención de Errores:**
    - Evita cambios no deseados si cometes un error (por ejemplo, olvidar una cláusula `WHERE` en un `DELETE`).
3. **Integridad de los Datos:**
    - Garantiza que las operaciones complejas se completen como una unidad lógica.

---

### **Resumen de Comandos**

| Acción                  | Oracle Database          | MySQL                     | SQL Server               |
|-------------------------|-------------------------|---------------------------|--------------------------|
| Iniciar Transacción      | Implícita               | `START TRANSACTION;`      | `BEGIN TRANSACTION;`     |
| Desactivar Autocommit    | No aplicable            | `SET AUTOCOMMIT=0;`       | `SET IMPLICIT_TRANSACTIONS ON;` |
| Confirmar Transacción    | `COMMIT;`               | `COMMIT;`                 | `COMMIT;`                |
| Revertir Transacción     | `ROLLBACK;`             | `ROLLBACK;`               | `ROLLBACK;`              |

---

### **Conclusión**

- **Transacciones Implícitas:** Oracle Database maneja transacciones de manera implícita, lo que permite revertir cambios en cualquier momento.
- **Modo de Autocommit:** MySQL y SQL Server confirman automáticamente las declaraciones SQL individuales a menos que se inicie explícitamente una transacción.
- **Desactivar Autocommit:** Es una buena práctica desactivar el modo de autocommit y ejecutar todas las declaraciones SQL dentro de transacciones para mantener el control sobre los cambios y prevenir errores.

En resumen, entender cómo manejan las transacciones los diferentes servidores de bases de datos te ayudará a desarrollar aplicaciones más robustas y confiables. 