# Creación de Proyectos en Node.js y Uso de Índices en MongoDB

Este documento explica cómo iniciar un proyecto en Node.js y cómo crear índices en MongoDB desde mongosh y Node.js.

---

# 1. ¿Cómo funciona la creación de proyectos en Node.js?

## 1.1 Inicializar el proyecto

```bash
mkdir mi_proyecto
cd mi_proyecto
npm init -y
```

Esto crea el archivo `package.json` con la configuración básica.

## 1.2 Instalar dependencias

```bash
npm install express mongodb
```

Esto agrega las dependencias y crea la carpeta `node_modules`.

## 1.3 Crear archivo principal

```javascript
// index.js
const express = require('express');
const app = express();

app.get('/', (req, res) => res.send('Hola mundo desde Node.js'));

app.listen(3000, () => console.log('Servidor corriendo en puerto 3000'));
```

## 1.4 Ejecutar el proyecto

```bash
node index.js
```

---

# 2. ¿Cómo se indexan campos en MongoDB?

Los índices permiten búsquedas eficientes sobre los campos de una colección.

---

# 3. Indexación desde MongoDB Shell (mongosh)

## 3.1 Índice ascendente

```javascript
db.usuarios.createIndex({ nombre: 1 });
```

## 3.2 Índice descendente

```javascript
db.usuarios.createIndex({ edad: -1 });
```

## 3.3 Índice compuesto

```javascript
db.usuarios.createIndex({ nombre: 1, edad: -1 });
```

## 3.4 Ver índices existentes

```javascript
db.usuarios.getIndexes();
```

---

# 4. Indexación desde Node.js

## 4.1 Crear un índice con el driver oficial

```javascript
const { MongoClient } = require('mongodb');

async function crearIndice() {
  const client = new MongoClient('mongodb://localhost:27017');
  await client.connect();

  const db = client.db("mi_base");
  const usuarios = db.collection("usuarios");

  await usuarios.createIndex({ nombre: 1 });

  console.log("Índice creado");

  await client.close();
}

crearIndice();
```

---

# 5. Casos comunes para usar índices

## 5.1 Búsquedas frecuentes

```javascript
db.usuarios.createIndex({ correo: 1 });
```

## 5.2 Ordenamientos frecuentes

```javascript
db.usuarios.createIndex({ fecha: -1 });
```

## 5.3 Consultas compuestas

```javascript
db.usuarios.createIndex({ categoria: 1, precio: -1 });
```

## 5.4 Índices únicos

```javascript
db.usuarios.createIndex({ correo: 1 }, { unique: true });
```

---

# 6. Buenas prácticas

- Indexar solo campos que se consultan frecuentemente.
- Usar índices compuestos para consultas con múltiples condiciones.
- No crear índices innecesarios para evitar impactos en el rendimiento.