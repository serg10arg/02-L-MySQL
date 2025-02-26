
### Ejercicio 12-1
Genera una unidad de trabajo para transferir $50 de la cuenta 123 a la cuenta 789. Necesitarás insertar dos filas en la tabla `transaction` y actualizar dos filas en la tabla `account`. Utiliza las siguientes definiciones de tabla y datos:

#### Tabla `Account`:
| account_id | avail_balance | last_activity_date       |
|------------|---------------|--------------------------|
| 123        | 500           | 2019-07-10 20:53:27      |
| 789        | 75            | 2019-06-22 15:18:35      |

#### Tabla `Transaction`:
| txn_id | txn_date       | account_id | txn_type_cd | amount |
|--------|----------------|------------|-------------|--------|
| 1001   | 2019-05-15     | 123        | C           | 500    |
| 1002   | 2019-06-01     | 789        | C           | 75     |

Utiliza `txn_type_cd = 'C'` para indicar un crédito (adicción) y `txn_type_cd = 'D'` para indicar un débito (sustracción).

### Paso 1: Actualizar la Tabla `account`

Primero, actualiza el `avail_balance` de ambas cuentas para reflejar la transferencia.

```sql
-- Inicia una transacción para asegurar la atomicidad
BEGIN TRANSACTION;

-- Actualiza la cuenta 123 (débito $50)
UPDATE account
SET avail_balance = avail_balance - 50,
    last_activity_date = NOW()
WHERE account_id = 123;

-- Actualiza la cuenta 789 (crédito $50)
UPDATE account
SET avail_balance = avail_balance + 50,
    last_activity_date = NOW()
WHERE account_id = 789;

-- Confirma la transacción
COMMIT;
```

### Paso 2: Insertar Filas en la Tabla `transaction`

Luego, inserta dos filas en la tabla `transaction` para registrar las transacciones de débito y crédito.

```sql
-- Inserta la transacción de débito para la cuenta 123
INSERT INTO transaction (txn_date, account_id, txn_type_cd, amount)
VALUES (NOW(), 123, 'D', 50);

-- Inserta la transacción de crédito para la cuenta 789
INSERT INTO transaction (txn_date, account_id, txn_type_cd, amount)
VALUES (NOW(), 789, 'C', 50);
```

### Script Completo en SQL

Combinando ambos pasos en un solo script:

```sql
-- Inicia una transacción para asegurar la atomicidad
BEGIN TRANSACTION;

-- Actualiza la cuenta 123 (débito $50)
UPDATE account
SET avail_balance = avail_balance - 50,
    last_activity_date = NOW()
WHERE account_id = 123;

-- Actualiza la cuenta 789 (crédito $50)
UPDATE account
SET avail_balance = avail_balance + 50,
    last_activity_date = NOW()
WHERE account_id = 789;

-- Inserta la transacción de débito para la cuenta 123
INSERT INTO transaction (txn_date, account_id, txn_type_cd, amount)
VALUES (NOW(), 123, 'D', 50);

-- Inserta la transacción de crédito para la cuenta 789
INSERT INTO transaction (txn_date, account_id, txn_type_cd, amount)
VALUES (NOW(), 789, 'C', 50);

-- Confirma la transacción
COMMIT;
```

### Explicación

1. **Gestión de Transacciones**:
    - `BEGIN TRANSACTION;`: Inicia una transacción para asegurar que todos los cambios se apliquen de manera atómica.
    - `COMMIT;`: Confirma la transacción, haciendo todos los cambios permanentes.

2. **Actualizar la Tabla `account`**:
    - Las sentencias `UPDATE` ajustan el `avail_balance` de ambas cuentas. El `last_activity_date` se actualiza al timestamp actual (`NOW()`).

3. **Insertar en la Tabla `transaction`**:
    - Dos sentencias `INSERT` registran las transacciones. Una para el débito (`txn_type_cd = 'D'`) de la cuenta 123 y otra para el crédito (`txn_type_cd = 'C'`) a la cuenta 789.

### Consideraciones Importantes

- **Atomicidad**: Utilizar transacciones asegura que todos los cambios se apliquen o ninguno, manteniendo la integridad de los datos.
- **Timestamps**: La función `NOW()` se utiliza para establecer el timestamp actual tanto para `last_activity_date` como para `txn_date`.
- **Manejo de Errores**: En un entorno de producción, deberías incluir manejo de errores para gestionar cualquier problema que surja durante la transacción.

Este script asegura que la transferencia se registre correctamente y mantenga la integridad de los saldos de las cuentas y el historial de transacciones.