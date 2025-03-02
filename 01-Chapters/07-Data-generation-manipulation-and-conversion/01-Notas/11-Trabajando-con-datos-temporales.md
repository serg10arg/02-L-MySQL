## Trabajando con Datos Temporales

De los tres tipos de datos discutidos en este capítulo (caracteres, numéricos y temporales), los datos temporales son los más complejos en cuanto a generación y manipulación. Parte de la complejidad de los datos temporales se debe a las diversas formas en que se puede describir una sola fecha y hora. Por ejemplo, la fecha en la que escribí este párrafo se puede describir de las siguientes maneras:
- Miércoles, 5 de junio de 2019
- 6/05/2019 2:14:56 P.M. EST
- 6/05/2019 19:14:56 GMT
- 1562019 (formato juliano)
- Fecha estelar [-4] 97026.79 14:14:56 (formato de Star Trek)

Aunque algunas de estas diferencias son simplemente una cuestión de formato, la mayor parte de la complejidad tiene que ver con tu marco de referencia, el cual exploramos en la siguiente sección.

## Manejo de Zonas Horarias

Debido a que las personas en todo el mundo prefieren que el mediodía coincida aproximadamente con el pico del sol en su ubicación, nunca ha habido un intento serio de obligar a todos a usar un reloj universal. En su lugar, el mundo se ha dividido en 24 secciones imaginarias, llamadas zonas horarias; dentro de una zona horaria particular, todos están de acuerdo en la hora actual, mientras que las personas en diferentes zonas horarias no. Aunque esto parece lo suficientemente simple, algunas regiones geográficas cambian su hora una hora dos veces al año (implementando lo que se conoce como horario de verano) y otras no, por lo que la diferencia horaria entre dos puntos en la Tierra podría ser de cuatro horas durante la mitad del año y de cinco horas durante la otra mitad del año. Incluso dentro de una sola zona horaria, diferentes regiones pueden o no adherirse al horario de verano, lo que causa que diferentes relojes en la misma zona horaria coincidan durante la mitad del año, pero difieran en una hora durante el resto del año.

Aunque la era de la computación ha exacerbado el problema, las personas han estado lidiando con las diferencias de zonas horarias desde los primeros días de la exploración naval. Para asegurar un punto de referencia común para la medición del tiempo, los navegantes del siglo XV ajustaban sus relojes a la hora del día en Greenwich, Inglaterra. Esto se conoció como el Tiempo Medio de Greenwich, o GMT. Todas las demás zonas horarias se pueden describir por la diferencia de horas con respecto al GMT; por ejemplo, la zona horaria del este de los Estados Unidos, conocida como Hora Estándar del Este, se puede describir como GMT -5:00, o cinco horas antes que el GMT.

Hoy en día, usamos una variación del GMT llamada Tiempo Universal Coordinado, o UTC, que se basa en un reloj atómico (o, para ser más precisos, el promedio del tiempo de 200 relojes atómicos en 50 ubicaciones en todo el mundo, que se refiere como Tiempo Universal). Tanto SQL Server como MySQL proporcionan funciones que devolverán la marca de tiempo UTC actual (`getutcdate()` para SQL Server y `utc_timestamp()` para MySQL).

La mayoría de los servidores de bases de datos por defecto usan la configuración de zona horaria del servidor en el que reside y proporcionan herramientas para modificar la zona horaria si es necesario. Por ejemplo, una base de datos utilizada para almacenar transacciones de la bolsa de valores de todo el mundo generalmente se configuraría para usar el tiempo UTC, mientras que una base de datos utilizada para almacenar transacciones en un establecimiento minorista particular podría usar la zona horaria del servidor.

MySQL mantiene dos configuraciones de zona horaria diferentes: una zona horaria global y una zona horaria de sesión, que puede ser diferente para cada usuario que haya iniciado sesión en una base de datos. Puedes ver ambas configuraciones a través de la siguiente consulta:

```sql
mysql> SELECT @@global.time_zone, @@session.time_zone;
+--------------------+---------------------+
| @@global.time_zone | @@session.time_zone |
+--------------------+---------------------+
| SYSTEM             | SYSTEM              |
+--------------------+---------------------+
1 row in set (0.00 sec)
```

Un valor de `SYSTEM` te dice que el servidor está usando la configuración de zona horaria del servidor en el que reside la base de datos.

Si estás sentado en una computadora en Zúrich, Suiza, y abres una sesión a través de la red a un servidor MySQL situado en Nueva York, es posible que desees cambiar la configuración de zona horaria para tu sesión, lo cual puedes hacer a través del siguiente comando:

```sql
mysql> SET time_zone = 'Europe/Zurich';
Query OK, 0 rows affected (0.18 sec)
```

Si vuelves a verificar las configuraciones de zona horaria, verás lo siguiente:

```sql
mysql> SELECT @@global.time_zone, @@session.time_zone;
+--------------------+---------------------+
| @@global.time_zone | @@session.time_zone |
+--------------------+---------------------+
| SYSTEM             | Europe/Zurich       |
+--------------------+---------------------+
1 row in set (0.00 sec)
```

Todas las fechas mostradas en tu sesión ahora se ajustarán a la hora de Zúrich.

Los usuarios de Oracle Database pueden cambiar la configuración de zona horaria para una sesión a través del siguiente comando:

```sql
ALTER SESSION SET TIME_ZONE = 'Europe/Zurich';
```

---

