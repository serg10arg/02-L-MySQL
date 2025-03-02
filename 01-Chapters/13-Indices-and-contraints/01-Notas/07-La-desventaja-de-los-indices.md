
---

**Las Desventajas de los Índices**

Si los índices son tan buenos, ¿por qué no indexar todo? Bueno, la clave para entender por qué tener más índices no es necesariamente algo bueno es tener en cuenta que cada índice es una tabla (un tipo especial de tabla, pero aún así una tabla). Por lo tanto, cada vez que se agrega o elimina una fila de una tabla, todos los índices en esa tabla deben modificarse. Cuando se actualiza una fila, cualquier índice en la columna o columnas que se vieron afectadas también debe modificarse. Por lo tanto, cuanto más índices tengas, más trabajo necesita hacer el servidor para mantener todos los objetos del esquema actualizados, lo que tiende a ralentizar las cosas.

Los índices también requieren espacio en disco, así como cierto cuidado por parte de tus administradores, por lo que la mejor estrategia es añadir un índice cuando surge una necesidad clara. Si necesitas un índice solo para propósitos especiales, como una rutina de mantenimiento mensual, siempre puedes añadir el índice, ejecutar la rutina y luego eliminar el índice hasta que lo necesites nuevamente. En el caso de almacenes de datos, donde los índices son cruciales durante las horas laborales mientras los usuarios ejecutan informes y consultas ad hoc pero son problemáticos cuando se cargan datos en el almacén durante la noche, es una práctica común eliminar los índices antes de cargar los datos y luego recrearlos antes de que el almacén abra para los negocios.

En general, debes esforzarte por no tener ni demasiados índices ni muy pocos. Si no estás seguro de cuántos índices deberías tener, puedes usar esta estrategia por defecto:
- Asegúrate de que todas las columnas de clave primaria estén indexadas (la mayoría de los servidores crean automáticamente índices únicos cuando creas restricciones de clave primaria). Para las claves primarias multicolumna, considera construir índices adicionales en un subconjunto de las columnas de clave primaria o en todas las columnas de clave primaria, pero en un orden diferente al de la definición de la restricción de clave primaria.
- Construye índices en todas las columnas que se referencian en las restricciones de clave foránea. Ten en cuenta que el servidor verifica para asegurarse de que no haya filas secundarias cuando se elimina un padre, por lo que debe emitir una consulta para buscar un valor en particular en la columna. Si no hay un índice en la columna, se debe escanear toda la tabla.
- Indexa cualquier columna que se utilizará frecuentemente para recuperar datos. La mayoría de las columnas de fechas son buenos candidatos, junto con columnas de cadenas cortas (de 2 a 50 caracteres).

Después de haber construido tu conjunto inicial de índices, intenta capturar consultas reales contra tus tablas, mira el plan de ejecución del servidor y modifica tu estrategia de indexación para adaptarse a las rutas de acceso más comunes.

---

