
### ¿Qué es una Clave Foránea?

Una clave foránea es una forma de conectar dos tablas en una base de datos. Imagina que tienes dos tablas: una tabla `person` que almacena información sobre personas y otra tabla `favorite_food` que almacena las comidas favoritas de esas personas.

### Ejemplo:

- **Tabla `person`**: contiene información sobre personas, como sus nombres y direcciones.
- **Tabla `favorite_food`**: contiene información sobre las comidas favoritas de las personas. Pero en lugar de repetir toda la información de la persona, solo guarda un identificador (`person_id`) que hace referencia a la persona en la tabla `person`.

### ¿Cómo Funciona la Clave Foránea?

La clave foránea en la tabla `favorite_food` es la columna `person_id`. Esto significa que para cada entrada en `favorite_food`, el `person_id` debe coincidir con un `person_id` existente en la tabla `person`. Esto asegura que las comidas favoritas que agregas están vinculadas a personas reales en la tabla `person`.

### Visualización:

1. **Tabla `person`**:
    - `person_id` | `name`
    - ----------- | ------
    - 1           | Alice
    - 2           | Bob

2. **Tabla `favorite_food`**:
    - `person_id` | `food`
    - ----------- | ------
    - 1           | Pizza
    - 2           | Sushi

### Restricción de Clave Foránea

La restricción de clave foránea garantiza que solo puedes agregar datos en `favorite_food` si existe una persona correspondiente en `person`. Por ejemplo, no puedes agregar "person_id 3" en `favorite_food` si "person_id 3" no existe en `person`.

### Ejemplo con Restricción de Clave Foránea:

- Si intentas agregar `person_id = 3` en `favorite_food` y no hay una fila en `person` con `person_id = 3`, recibirás un error.

### Resumen:

- **Propósito**: Asegurar la integridad referencial, es decir, que los datos en una tabla (favorite_food) estén relacionados con datos válidos en otra tabla (person).
- **Funcionamiento**: La restricción impide agregar datos no válidos en la tabla `favorite_food` que no correspondan a una persona existente en la tabla `person`.

