
---

### **Finalizando una Transacción**

Una vez que una transacción ha comenzado, ya sea explícitamente mediante el comando `START TRANSACTION` o implícitamente por el servidor de base de datos, debes finalizarla explícitamente para que los cambios se vuelvan permanentes o se deshagan.

#### Comandos para Finalizar una Transacción:
1. **`COMMIT`:**
    - Confirma los cambios realizados durante la transacción.
    - Los cambios se vuelven permanentes en la base de datos.
    - Libera los recursos (como bloqueos de página o fila) utilizados durante la transacción.

2. **`ROLLBACK`:**
    - Deshace todos los cambios realizados durante la transacción.
    - Los datos vuelven a su estado anterior a la transacción.
    - Libera los recursos utilizados por la sesión.

---

### **Escenarios que Finalizan una Transacción**

Además de los comandos `COMMIT` y `ROLLBACK`, hay varios escenarios en los que una transacción puede finalizar, ya sea como resultado de tus acciones o por eventos externos:

1. **El Servidor se Apaga:**
    - Si el servidor se apaga durante una transacción, esta se revertirá automáticamente cuando el servidor se reinicie.
    - **Ejemplo:** Si estabas realizando una transferencia de fondos y el servidor se apaga antes de confirmar la transacción, los cambios se desharán.

2. **Declaraciones de Esquema SQL:**
    - Si emites una declaración de esquema SQL (como `ALTER TABLE`), la transacción actual se confirmará automáticamente y se iniciará una nueva transacción.
    - **Ejemplo:**
      ```sql
      START TRANSACTION;
      UPDATE account SET avail_balance = avail_balance - 500 WHERE account_id = 9988;
      ALTER TABLE account ADD COLUMN new_column INT;  -- Confirma la transacción actual
      UPDATE account SET avail_balance = avail_balance + 500 WHERE account_id = 9989;
      COMMIT;
      ```
    - **Nota:** Las alteraciones de esquema no pueden revertirse, por lo que deben ejecutarse fuera de una transacción.

3. **Iniciar una Nueva Transacción:**
    - Si emites otro comando `START TRANSACTION` o `BEGIN TRANSACTION`, la transacción anterior se confirmará automáticamente.
    - **Ejemplo:**
      ```sql
      START TRANSACTION;
      UPDATE account SET avail_balance = avail_balance - 500 WHERE account_id = 9988;
      START TRANSACTION;  -- Confirma la transacción anterior
      UPDATE account SET avail_balance = avail_balance + 500 WHERE account_id = 9989;
      COMMIT;
      ```

4. **Detección de Bloqueo Mutuo:**
    - Si el servidor detecta un bloqueo mutuo (deadlock), finalizará una de las transacciones involucradas y la revertirá.
    - **Ejemplo:**
        - **Transacción A:** Bloquea la tabla `account` y espera un bloqueo en la tabla `transaction`.
        - **Transacción B:** Bloquea la tabla `transaction` y espera un bloqueo en la tabla `account`.
        - El servidor elige una transacción para revertir (generalmente la que ha realizado menos trabajo) y permite que la otra continúe.

---

### **Detección de Bloqueo Mutuo**

Un **bloqueo mutuo** ocurre cuando dos o más transacciones están esperando recursos que la otra transacción tiene bloqueados. Esto puede llevar a una espera indefinida, por lo que el servidor de base de datos debe detectar y resolver estos bloqueos.

#### Ejemplo de Bloqueo Mutuo:
1. **Transacción A:**
    - Bloquea la tabla `account` y espera un bloqueo en la tabla `transaction`.
2. **Transacción B:**
    - Bloquea la tabla `transaction` y espera un bloqueo en la tabla `account`.

#### Resolución:
- El servidor detecta el bloqueo mutuo y elige una transacción para revertir (generalmente la que ha realizado menos trabajo).
- La transacción revertida recibe un mensaje de error.
- **Ejemplo en MySQL:**
  ```
  Error 1213: Deadlock found when trying to get lock; try restarting transaction
  ```

#### Recomendaciones:
- **Reintentar la Transacción:** Es una práctica común reintentar una transacción que ha sido revertida debido a un bloqueo mutuo.
- **Prevención de Bloqueos Mutuos:**
    - Acceder a los recursos en un orden consistente (por ejemplo, siempre modificar `account` antes que `transaction`).
    - Reducir el tiempo de bloqueo manteniendo las transacciones lo más cortas posible.

---

### **Conclusión**

- **Finalización de Transacciones:** Puedes finalizar una transacción con `COMMIT` (confirmar cambios) o `ROLLBACK` (deshacer cambios).
- **Escenarios de Finalización:** Incluyen el apagado del servidor, declaraciones de esquema SQL, inicio de una nueva transacción y detección de bloqueos mutuos.
- **Bloqueos Mutuos:** Son situaciones en las que dos transacciones esperan recursos bloqueados por la otra. El servidor revierte una de las transacciones para resolver el bloqueo.

En resumen, entender cómo finalizan las transacciones y cómo manejar situaciones como los bloqueos mutuos es crucial para garantizar la integridad y el rendimiento de tus aplicaciones. 