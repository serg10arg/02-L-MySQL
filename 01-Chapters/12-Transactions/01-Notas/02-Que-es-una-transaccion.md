
---

### **¿Qué es una Transacción?**

Una **transacción** es un conjunto de operaciones SQL que se ejecutan como una unidad lógica de trabajo. Las transacciones garantizan que todas las operaciones se completen con éxito o que ninguna se aplique, lo que mantiene la integridad de los datos.

#### Propiedades de las Transacciones (ACID):
1. **Atomicidad:** Todas las operaciones de la transacción se completan o ninguna.
2. **Consistencia:** La transacción lleva la base de datos de un estado válido a otro.
3. **Aislamiento:** Las transacciones concurrentes no interfieren entre sí.
4. **Durabilidad:** Los cambios realizados por una transacción confirmada son permanentes.

---

### **Ejemplo de Transacción: Transferencia de Fondos**

El texto proporciona un ejemplo en el que se transfieren $500 de una cuenta de ahorros a una cuenta corriente. Este proceso debe ser atómico: o ambas operaciones tienen éxito, o ninguna.

#### Pseudocódigo:
```sql
START TRANSACTION;

/* Retira dinero de la primera cuenta, asegurándose de que el saldo sea suficiente */
UPDATE account SET avail_balance = avail_balance - 500
WHERE account_id = 9988
  AND avail_balance > 500;

IF <exactamente una fila fue actualizada por la declaración anterior> THEN
  /* Deposita dinero en la segunda cuenta */
  UPDATE account SET avail_balance = avail_balance + 500
    WHERE account_id = 9989;

  IF <exactamente una fila fue actualizada por la declaración anterior> THEN
    /* Todo funcionó, haz los cambios permanentes */
    COMMIT;
  ELSE
    /* Algo salió mal, deshaz todos los cambios en esta transacción */
    ROLLBACK;
  END IF;
ELSE
  /* Fondos insuficientes o error encontrado durante la actualización */
  ROLLBACK;
END IF;
```

#### Explicación:
1. **Inicio de la Transacción:**
    - `START TRANSACTION;` inicia una nueva transacción.

2. **Retiro de Fondos:**
    - Se intenta retirar $500 de la cuenta de ahorros (`account_id = 9988`).
    - La condición `avail_balance > 500` asegura que haya fondos suficientes.

3. **Depósito de Fondos:**
    - Si el retiro es exitoso, se intenta depositar $500 en la cuenta corriente (`account_id = 9989`).

4. **Confirmación o Reversión:**
    - Si ambas operaciones tienen éxito, se confirman los cambios (`COMMIT`).
    - Si alguna operación falla, se deshacen todos los cambios (`ROLLBACK`).

---

### **Importancia de las Transacciones**

Las transacciones son esenciales para garantizar la integridad de los datos en operaciones complejas. En el ejemplo de la transferencia de fondos:
- **Sin Transacciones:** Podría ocurrir que el dinero se retire de la cuenta de ahorros pero no se deposite en la cuenta corriente, lo que resultaría en una pérdida de fondos.
- **Con Transacciones:** Si alguna operación falla, todas las operaciones se deshacen, garantizando que los fondos no se pierdan.

---

### **Manejo de Fallos**

1. **Fallos del Servidor:**
    - Si el servidor se apaga antes de confirmar o revertir la transacción, los cambios se deshacen automáticamente cuando el servidor se reinicia.
    - Esto garantiza que no se apliquen cambios parciales.

2. **Durabilidad:**
    - Si el servidor se apaga después de confirmar la transacción pero antes de escribir los cambios en el disco, el servidor debe volver a aplicar los cambios cuando se reinicie.
    - Esto garantiza que los cambios confirmados sean permanentes.

---

### **Liberación de Recursos**

Cuando una transacción se completa (ya sea con `COMMIT` o `ROLLBACK`), todos los recursos adquiridos (como bloqueos de escritura) se liberan. Esto permite que otros usuarios accedan a los datos modificados.

---

### **Conclusión**

- **Transacciones:** Son un mecanismo esencial para garantizar la integridad de los datos en operaciones complejas.
- **Atomicidad:** Asegura que todas las operaciones de la transacción se completen o ninguna.
- **Durabilidad:** Garantiza que los cambios confirmados sean permanentes, incluso en caso de fallos del servidor.

En resumen, las transacciones son una herramienta poderosa para manejar operaciones que involucran múltiples pasos, asegurando que los datos siempre estén en un estado consistente. 