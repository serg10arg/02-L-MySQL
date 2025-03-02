## Generación de Datos Temporales

Puedes generar datos temporales mediante cualquiera de los siguientes medios:
- Copiar datos de una columna existente de fecha, datetime o tiempo
- Ejecutar una función incorporada que devuelva una fecha, datetime o tiempo
- Construir una representación en cadena de los datos temporales para que sean evaluados por el servidor

Para usar el último método, necesitarás entender los diversos componentes utilizados en el formato de fechas.

### Representaciones en Cadena de Datos Temporales

La Tabla 2-4 en el Capítulo 2 presentó los componentes de fecha más populares; para refrescar tu memoria, la Tabla 7-2 muestra estos mismos componentes.

### Tabla 7-2. Componentes de Formato de Fecha

| Componente | Definición          | Rango                         |
|------------|---------------------|-------------------------------|
| YYYY       | Año, incluyendo siglo | 1000 a 9999                   |
| MM         | Mes                 | 01 (Enero) a 12 (Diciembre)   |
| DD         | Día                 | 01 a 31                       |
| HH         | Hora                | 00 a 23                       |
| HHH        | Horas (transcurridas) | -838 a 838                    |
| MI         | Minuto              | 00 a 59                       |
| SS         | Segundo             | 00 a 59                       |

Para construir una cadena que el servidor pueda interpretar como una fecha, datetime o tiempo, necesitas juntar los diversos componentes en el orden mostrado en la Tabla 7-3.

### Tabla 7-3. Componentes de Fecha Requeridos

| Tipo      | Formato Predeterminado         |
|-----------|---------------------------------|
| date      | YYYY-MM-DD                      |
| datetime  | YYYY-MM-DD HH:MI:SS             |
| timestamp | YYYY-MM-DD HH:MI:SS             |
| time      | HHH:MI:SS                       |

Así, para llenar una columna `datetime` con las 3:30 P.M. del 17 de septiembre de 2019, necesitarás construir la siguiente cadena:
```
'2019-09-17 15:30:00'
```

Si el servidor espera un valor `datetime`, como al actualizar una columna `datetime` o al llamar una función incorporada que toma un argumento `datetime`, puedes proporcionar una cadena formateada correctamente con los componentes de fecha requeridos, y el servidor hará la conversión por ti. Por ejemplo, aquí hay una declaración utilizada para modificar la fecha de devolución de un alquiler de película:
```sql
UPDATE rental
SET return_date = '2019-09-17 15:30:00'
WHERE rental_id = 99999;
```

El servidor determina que la cadena proporcionada en la cláusula `SET` debe ser un valor `datetime`, ya que la cadena se está utilizando para llenar una columna `datetime`. Por lo tanto, el servidor intentará convertir la cadena por ti desglosándola en los seis componentes (año, mes, día, hora, minuto, segundo) incluidos en el formato predeterminado `datetime`.

### Conversiones de Cadena a Fecha

Si el servidor no está esperando un valor `datetime` o si te gustaría representar el `datetime` utilizando un formato no predeterminado, necesitarás decirle al servidor que convierta la cadena a `datetime`. Por ejemplo, aquí hay una consulta simple que devuelve un valor `datetime` usando la función `cast()`:
```sql
mysql> SELECT CAST('2019-09-17 15:30:00' AS DATETIME);
+-----------------------------------------+
| CAST('2019-09-17 15:30:00' AS DATETIME) |
+-----------------------------------------+
| 2019-09-17 15:30:00                     |
+-----------------------------------------+
1 row in set (0.00 sec)
```

Cubrimos la función `cast()` al final de este capítulo. Aunque este ejemplo demuestra cómo construir valores `datetime`, la misma lógica se aplica a los tipos `date` y `time` también. La siguiente consulta usa la función `cast()` para generar un valor `date` y un valor `time`:
```sql
mysql> SELECT CAST('2019-09-17' AS DATE) date_field,
    ->   CAST('108:17:57' AS TIME) time_field;
+------------+------------+
| date_field | time_field |
+------------+------------+
| 2019-09-17 | 108:17:57  |
+------------+------------+
1 row in set (0.00 sec)
```

Por supuesto, podrías convertir explícitamente tus cadenas incluso cuando el servidor esté esperando un valor `date`, `datetime` o `time`, en lugar de permitir que el servidor haga una conversión implícita.

Cuando las cadenas se convierten en valores temporales, ya sea explícitamente o implícitamente, debes proporcionar todos los componentes de fecha en el orden requerido. Aunque algunos servidores son bastante estrictos en cuanto al formato de la fecha, el servidor MySQL es bastante flexible con respecto a los separadores utilizados entre los componentes. Por ejemplo, MySQL aceptará todas las siguientes cadenas como representaciones válidas de las 3:30 P.M. del 17 de septiembre de 2019:
- '2019-09-17 15:30:00'
- '2019/09/17 15:30:00'
- '2019,09,17,15,30,00'
- '20190917153000'

Aunque esto te da un poco más de flexibilidad, puedes encontrarte tratando de generar un valor temporal sin los componentes de fecha predeterminados; la siguiente sección demuestra una función incorporada que es mucho más flexible que la función `cast()`.

### Funciones para Generar Fechas

Si necesitas generar datos temporales a partir de una cadena y la cadena no está en la forma adecuada para usar la función `cast()`, puedes usar una función incorporada que te permite proporcionar una cadena de formato junto con la cadena de fecha. MySQL incluye la función `str_to_date()` para este propósito. Supongamos, por ejemplo, que obtienes la cadena '17 de septiembre de 2019' de un archivo y necesitas usarla para actualizar una columna de fecha. Dado que la cadena no está en el formato YYYY-MM-DD requerido, puedes usar `str_to_date()` en lugar de reformatear la cadena para que puedas usar la función `cast()`, como en:
```sql
UPDATE rental
SET return_date = STR_TO_DATE('September 17, 2019', '%M %d, %Y')
WHERE rental_id = 99999;
```

El segundo argumento en la llamada a `str_to_date()` define el formato de la cadena de fecha, en este caso, un nombre de mes (%M), un día numérico (%d) y un año numérico de cuatro dígitos (%Y). Aunque hay más de 30 componentes de formato reconocidos, la Tabla 7-4 define los aproximadamente doce componentes más comúnmente utilizados.

### Tabla 7-4. Componentes de Formato de Fecha

| Componente de Formato | Descripción                           |
|-----------------------|---------------------------------------|
| %M                    | Nombre del mes (Enero a Diciembre)    |
| %m                    | Mes numérico (01 a 12)                |
| %d                    | Día numérico (01 a 31)                |
| %j                    | Día del año (001 a 366)               |
| %W                    | Nombre del día de la semana (Domingo a Sábado) |
| %Y                    | Año, numérico de cuatro dígitos       |
| %y                    | Año, numérico de dos dígitos          |
| %H                    | Hora (00 a 23)                        |
| %h                    | Hora (01 a 12)                        |
| %i                    | Minutos (00 a 59)                     |
| %s                    | Segundos (00 a 59)                    |
| %f                    | Microsegundos (000000 a 999999)       |
| %p                    | A.M. o P.M.                           |

La función `str_to_date()` devuelve un valor `datetime`, `date` o `time` dependiendo del contenido de la cadena de formato. Por ejemplo, si la cadena de formato incluye solo %H, %i y %s, entonces se devolverá un valor de tiempo.

Los usuarios de Oracle Database pueden usar la función `to_date()` de la misma manera que la función `str_to_date()` de MySQL. SQL Server incluye una función `convert()` que no es tan flexible como MySQL y Oracle Database; en lugar de proporcionar una cadena de formato personalizada, tu cadena de fecha debe cumplir con uno de los 21 formatos predefinidos.

Si estás tratando de generar la fecha/hora actual, entonces no necesitarás construir una cadena, porque las siguientes funciones incorporadas accederán al reloj del sistema y devolverán la fecha y/o hora actual como una cadena para ti:

```sql
mysql> SELECT CURRENT_DATE(), CURRENT_TIME(), CURRENT_TIMESTAMP();
+----------------+----------------+---------------------+
| CURRENT_DATE() | CURRENT_TIME() | CURRENT_TIMESTAMP() |
+----------------+----------------+---------------------+
| 2019-06-05     | 16:54:36       | 2019-06-05 16:54:36 |
+----------------+----------------+---------------------+
1 row in set (0.12 sec)
```

Los valores devueltos por estas funciones están en el formato predeterminado para el tipo temporal que se devuelve. Oracle Database incluirá `current_date()` y `current_timestamp()` pero no `current_time()`, y Microsoft SQL Server incluye solo la función