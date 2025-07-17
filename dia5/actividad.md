# Workshop Incremental: Indexación de Campos en MongoDB

## Tabla de Contenidos

1. Introducción teórica
2. Preparación del entorno y dataset
3. Indexación básica
4. Indexación compuesta
5. Índices únicos
6. Índices sobre campos embebidos y arrays
7. Análisis de uso de índices
8. Eliminación de índices
9. Taller de ejercicios

------

## 1. Introducción teórica

### ¿Qué es un índice en MongoDB?

Un índice en MongoDB es una estructura de datos especial que almacena una porción del conjunto de datos de una colección de forma ordenada. Es similar al índice de un libro: permite que las consultas encuentren datos rápidamente sin tener que recorrer todos los documentos.

### ¿Por qué usar índices?

- **Mejoran el rendimiento** de las consultas.
- Son esenciales para operaciones como `find()`, `sort()`, y `aggregate`.
- Sin índices adecuados, MongoDB realizará un **collection scan**, lo cual es ineficiente.

------

## 2. Preparación del entorno y dataset

Creamos la base de datos y la colección con datos simulados:

```
use taller_indexacion;

db.usuarios.drop();

db.usuarios.insertMany([
  { nombre: "Carlos", edad: 28, ciudad: "Bogotá", correo: "carlos@example.com", intereses: ["fútbol", "viajes"] },
  { nombre: "Ana", edad: 34, ciudad: "Medellín", correo: "ana@example.com", intereses: ["lectura", "teatro"] },
  { nombre: "Luis", edad: 22, ciudad: "Bogotá", correo: "luis@example.com", intereses: ["música", "videojuegos"] },
  { nombre: "María", edad: 40, ciudad: "Cali", correo: "maria@example.com", intereses: ["pintura", "fútbol"] },
  { nombre: "Julián", edad: 30, ciudad: "Bucaramanga", correo: "julian@example.com", intereses: ["cine", "música"] },
  { nombre: "Paula", edad: 25, ciudad: "Bogotá", correo: "paula@example.com", intereses: ["teatro", "viajes"] },
  { nombre: "Camilo", edad: 38, ciudad: "Medellín", correo: "camilo@example.com", intereses: ["fotografía", "lectura"] }
]);
```

------

## 3. Indexación básica

### Crear un índice en un solo campo

```
db.usuarios.createIndex({ ciudad: 1 });
```

Este índice mejora las consultas que filtran por ciudad:

```
db.usuarios.find({ ciudad: "Bogotá" });
```

------

## 4. Indexación compuesta

Un índice compuesto mejora el rendimiento cuando se filtra por múltiples campos:

```
db.usuarios.createIndex({ ciudad: 1, edad: -1 });
```

Consulta optimizada:

```
db.usuarios.find({ ciudad: "Bogotá", edad: { $gt: 25 } });
```

------

## 5. Índices únicos

Evitan duplicados en campos como correos electrónicos:

```
db.usuarios.createIndex({ correo: 1 }, { unique: true });
```

Probar insert duplicado:

```
db.usuarios.insertOne({ nombre: "Pedro", edad: 29, ciudad: "Cali", correo: "carlos@example.com" });
```

Debería lanzar un error por duplicidad.

------

## 6. Índices sobre campos embebidos y arrays

### Campo embebido

Si tienes documentos anidados como:

```
db.usuarios.updateMany({}, { $set: { contacto: { telefono: "3001234567", direccion: "Desconocida" } } });
db.usuarios.createIndex({ "contacto.telefono": 1 });
```

Consulta optimizada:

```
db.usuarios.find({ "contacto.telefono": "3001234567" });
```

### Índices sobre arrays

MongoDB crea automáticamente índices multikey:

```
db.usuarios.createIndex({ intereses: 1 });
```

Consulta:

```
db.usuarios.find({ intereses: "fútbol" });
```

------

## 7. Análisis de uso de índices

Para verificar si se está usando un índice, usa `.explain()`:

```
db.usuarios.find({ ciudad: "Bogotá" }).explain("executionStats");
```

Revisar `"stage": "IXSCAN"` y `"totalKeysExamined"`.

------

## 8. Eliminación de índices

### Ver todos los índices

```
db.usuarios.getIndexes();
```

### Eliminar un índice

```
db.usuarios.dropIndex("ciudad_1");
```

------

## 9. Taller de ejercicios

Resuelve los siguientes ejercicios usando `mongosh`.

### Ejercicio 1

Crea un índice sobre el campo `edad` y realiza una consulta de usuarios mayores de 30 años.

### Ejercicio 2

Crea un índice único sobre el campo `nombre`. Intenta insertar un duplicado y verifica el error.

### Ejercicio 3

Agrega un campo embebido llamado `perfil` con los subcampos `ocupacion` y `nivel_estudios`. Crea un índice sobre `perfil.ocupacion` y realiza una consulta.

### Ejercicio 4

Crea un índice multikey sobre un array llamado `habilidades` y realiza una consulta que lo use.

### Ejercicio 5

Elimina el índice compuesto `{ ciudad: 1, edad: -1 }` y crea uno nuevo que sea `{ edad: 1, ciudad: 1 }`. Explica cuál sería más útil si la mayoría de consultas filtran primero por edad.