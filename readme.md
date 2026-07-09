# [Nombre] [Carné]

## Indicaciones

Recientemente, se utilizó AI para crear un sistema de gestion de una biblioteca, el cual ha generado varios errores, su trabajo es arreglarlo. Dado el siguiente caso de uso, explique y/o resuelva cada problema según se le pida.

---

## Consideraciones

La libreria crea automaticamente un correo con los nombres de la persona

---

## Problemas

### 1. Filtro por autor y género (10%)

QA ha reportado que el endpoint para obtener los libros puede filtrar por **autor** y por **género**, o por cualquiera de los dos de manera individual.

Actualmente:

- Filtrar únicamente por autor funciona correctamente.
- Filtrar únicamente por género funciona correctamente.
- Filtrar por **autor y género al mismo tiempo** provoca que el servidor falle.

**Instrucción:** Explique la causa del problema y resuélvalo.

# Respuesta
- El repositorio estaba recibiendo dos strings no un enum, el genero es un enum por ende se cambia el tipo a enum, y en el servicio se envia el Genre.valueOf(genre), y estaban al revez.

---

### 2. Error al volver a prestar un libro (10%)

Un usuario reportó que al pedir prestado el libro **The Selfish Gene**, devolverlo e intentar pedirlo prestado nuevamente, el servidor falla.

**Instrucción:** Explique la causa del problema y resuélvalo.

# Respuesta
- Al devolver el libro cuando se aumentama el numero disponible no se cambiaba su estado de disponible a true, dando error al querer pedirlo de regreso.

---

### 3. Cantidad de libros por género (10%)

Existe un endpoint que devuelve la cantidad de libros disponibles por género. Sin embargo, actualmente dicho endpoint falla.

**Instrucción:** Explique la causa del problema y resuélvalo.

# Respuesta
- Uno de los libros no tenia genero y regresaba NULL cuando se intentaba sacar su nombre daba error, se hizo validacion de si su genero viene y no es null, si es null se crea un fallback

---

### 4. Error al consultar un libro por ID (10%)

Un miembro del equipo de frontend reporta que la siguiente llamada falla:

```http
GET /books?id=ed16ed1e-7017-4697-a08a-d28c09a74acf
```

**Instrucción:** Explique la causa del problema.

# Respuesta
- En el endpoint no se tiene que enviar como query param, se tiene que enviar como path variable, lo correcto seria asi:
```http
GET /books/ed16ed1e-7017-4697-a08a-d28c09a74acf
```

---

### 5. Error al crear un libro (10%)

QA ha reportado que el siguiente payload enviado al endpoint `POST /books` provoca un error:

```json
{
  "title": "Clean Code",
  "author": "Robert C. Martin",
  "genre": "classic",
  "isbn": "978-0132350884",
  "available": true,
  "availableCount": 5
}
```

**Instrucción:** Explique la causa del problema.

# Respuesta 
- El genero "classic" tiene que ir en mayusculas, todos los generos se envian en mayusculas, seria "CLASSIC"

---

### 6. Devolución de libros no prestados (20%)

QA ha reportado que un usuario es capaz de devolver libros que nunca ha solicitado en préstamo.

**Instrucción:**

- Confirme si este comportamiento es realmente posible.
- Si es posible, explique la causa y resuelva el problema.
- Si no es posible, explique por qué, haciendo referencia al código correspondiente.

# Respuesta
- Si, si es posible, la causa es porque al devolver un libro no se esta verificando que exista un registro de que se presto
- solo aumenta la disponibilidad.
- Ahora se trae el ultimo registro, si no hay registro no deja regresarlo, si si hay y el ultimo no es un BORROW tampoco deja

---