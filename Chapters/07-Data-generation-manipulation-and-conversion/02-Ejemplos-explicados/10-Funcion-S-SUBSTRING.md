
### La Sentencia SQL:
```sql
SELECT SUBSTRING('goodbye cruel world', 9, 5);
```

### ¿Qué hace esta sentencia?

1. **SELECT**: Es una palabra clave en SQL que se utiliza para seleccionar datos de una base de datos. En este caso, estamos seleccionando el resultado de una función llamada `SUBSTRING`.

2. **SUBSTRING**: Es una función en SQL que se utiliza para extraer una parte de una cadena de texto. La función `SUBSTRING` toma tres argumentos:
    - **Primer argumento**: La cadena de texto de la cual queremos extraer una parte. En este caso, la cadena es `'goodbye cruel world'`.
    - **Segundo argumento**: La posición inicial desde donde queremos empezar a extraer. En este caso, es `9`.
    - **Tercer argumento**: La longitud de la subcadena que queremos extraer. En este caso, es `5`.

### Desglosando los argumentos:

- **Cadena de texto**: `'goodbye cruel world'`
    - Esta es la cadena completa de la cual queremos extraer una parte.

- **Posición inicial**: `9`
    - Esto indica que empezaremos a extraer desde el noveno carácter de la cadena. En SQL, las posiciones empiezan desde 1, no desde 0.

- **Longitud**: `5`
    - Esto indica que queremos extraer 5 caracteres a partir de la posición inicial.

### ¿Cómo se calcula la subcadena?

Vamos a contar los caracteres en la cadena `'goodbye cruel world'`:

```
1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
g  o  o  d  b  y  e     c  r  u  e  l     w  o  r  l  d
```

- El noveno carácter es `'c'`.
- A partir de `'c'`, extraemos 5 caracteres: `'c'`, `'r'`, `'u'`, `'e'`, `'l'`.

Por lo tanto, la subcadena extraída es `'cruel'`.

### Resultado:
```sql
+----------------------------------------+
| SUBSTRING('goodbye cruel world', 9, 5) |
+----------------------------------------+
| cruel                                  |
+----------------------------------------+
```

### Resumen:
- **`SUBSTRING`** es una función que extrae una parte de una cadena de texto.
- En este caso, extrajimos 5 caracteres a partir del noveno carácter de la cadena `'goodbye cruel world'`, lo que nos dio como resultado `'cruel'`.

