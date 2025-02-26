
---

### **Transacciones**

Una **transacción** es un conjunto de operaciones SQL que se ejecutan como una unidad lógica de trabajo. Las transacciones garantizan que todas las operaciones se completen con éxito o que ninguna se aplique, lo que mantiene la integridad de los datos.

#### Características de las Transacciones (ACID):
1. **Atomicidad:** Todas las operaciones de la transacción se completan o ninguna.
2. **Consistencia:** La transacción lleva la base de datos de un estado válido a otro.
3. **Aislamiento:** Las transacciones concurrentes no interfieren entre sí.
4. **Durabilidad:** Los cambios realizados por una transacción confirmada son permanentes.

#### Ejemplo de Transacción:
```sql
START TRANSACTION;

UPDATE cuenta SET saldo = saldo - 100 WHERE id = 1;
UPDATE cuenta SET saldo = saldo + 100 WHERE id = 2;

COMMIT;
```

- **Explicación:**
    - Si ambas operaciones tienen éxito, se confirman los cambios (`COMMIT`).
    - Si alguna operación falla, se deshacen todos los cambios (`ROLLBACK`).

---

### **Bases de Datos Multiusuario**

En un entorno multiusuario, múltiples usuarios pueden acceder y modificar la base de datos simultáneamente. Esto plantea desafíos para mantener la consistencia y la integridad de los datos.

#### Escenario:
- **Informe de Actividad:** Un usuario ejecuta un informe que resume la actividad de alquiler de películas.
- **Actividades Concurrentes:**
    - Un cliente alquila una película.
    - Un cliente devuelve una película con retraso y paga una tarifa.
    - Se agregan nuevas películas al inventario.

#### Problema:
- ¿Qué datos deben aparecer en el informe si los datos subyacentes están siendo modificados simultáneamente?

---

### **Bloqueos**

Los **bloqueos** son mecanismos que el servidor de la base de datos utiliza para controlar el acceso concurrente a los datos. Evitan conflictos entre usuarios que leen y modifican los mismos datos.

#### Estrategias de Bloqueo:
1. **Bloqueos de Lectura/Escritura:**
    - **Escritores:** Deben solicitar un bloqueo de escritura para modificar datos.
    - **Lectores:** Deben solicitar un bloqueo de lectura para consultar datos.
    - **Ventaja:** Garantiza la consistencia de los datos.
    - **Desventaja:** Puede causar largos tiempos de espera si hay muchas solicitudes concurrentes.

2. **Versionado:**
    - **Escritores:** Deben solicitar un bloqueo de escritura para modificar datos.
    - **Lectores:** No necesitan bloqueos; el servidor garantiza una vista consistente de los datos desde el inicio de la consulta.
    - **Ventaja:** Reduce los tiempos de espera para los lectores.
    - **Desventaja:** Puede ser problemático con consultas de larga duración y modificaciones frecuentes.

#### Sistemas de Bases de Datos:
- **Microsoft SQL Server:** Usa bloqueos de lectura/escritura.
- **Oracle Database:** Usa versionado.
- **MySQL:** Usa ambos enfoques, dependiendo del motor de almacenamiento.

---

### **Granularidades de Bloqueo**

La **granularidad de bloqueo** se refiere al nivel de detalle al que se aplican los bloqueos. Puede ser a nivel de tabla, página o fila.

#### Niveles de Granularidad:
1. **Bloqueos de Tabla:**
    - Bloquea toda la tabla.
    - **Ventaja:** Fácil de implementar y gestionar.
    - **Desventaja:** Puede causar largos tiempos de espera en entornos con alta concurrencia.

2. **Bloqueos de Página:**
    - Bloquea una página de memoria (generalmente de 2 KB a 16 KB).
    - **Ventaja:** Mayor concurrencia que los bloqueos de tabla.
    - **Desventaja:** Aún puede causar contención en páginas muy solicitadas.

3. **Bloqueos de Fila:**
    - Bloquea una sola fila de una tabla.
    - **Ventaja:** Máxima concurrencia; múltiples usuarios pueden modificar diferentes filas de la misma tabla.
    - **Desventaja:** Mayor sobrecarga de gestión.

#### Sistemas de Bases de Datos:
- **Microsoft SQL Server:** Usa bloqueos de página, fila y tabla.
- **Oracle Database:** Usa solo bloqueos de fila.
- **MySQL:** Usa bloqueos de tabla, página o fila, dependiendo del motor de almacenamiento.

---

### **Impacto en los Informes**

Cuando ejecutas un informe en un entorno multiusuario, los datos que aparecen en el informe dependen de la estrategia de bloqueo y granularidad utilizada por el servidor de la base de datos.

1. **Enfoque de Bloqueos de Lectura/Escritura:**
    - El informe refleja el estado de la base de datos cuando se otorga el bloqueo de lectura.
    - Si hay muchas modificaciones concurrentes, el informe puede retrasarse.

2. **Enfoque de Versionado:**
    - El informe refleja el estado de la base de datos cuando comienza la consulta.
    - Las modificaciones concurrentes no afectan los datos del informe.

---

### **Conclusión**

- **Transacciones:** Son esenciales para garantizar la integridad de los datos en operaciones complejas.
- **Bases de Datos Multiusuario:** Requieren mecanismos como bloqueos para manejar el acceso concurrente.
- **Bloqueos:** Controlan el acceso a los datos para evitar conflictos entre usuarios.
- **Granularidades de Bloqueo:** Determinan el nivel de detalle al que se aplican los bloqueos, afectando la concurrencia y el rendimiento.

En resumen, entender cómo funcionan las transacciones, los bloqueos y las granularidades de bloqueo es crucial para diseñar y gestionar bases de datos en entornos multiusuario.