# 🧪 Workshop: Validación de Datos en MongoDB

## Tabla de Contenidos

1. Fundamentos de Validación en MongoDB
2. Nivel 1 – Validación básica por tipo de dato
3. Nivel 2 – Validación con restricciones
4. Nivel 3 – Validación condicional y con expresiones
5. Nivel 4 – Uso combinado de validaciones
6. Nivel 5 – Validación de documentos embebidos y arreglos
7. Taller Final: Ejercicios para desarrollar

------

## 1. Fundamentos de Validación en MongoDB

MongoDB permite validar la estructura y contenido de los documentos mediante **esquemas de validación**, definidos al momento de crear la colección. Utiliza especificaciones compatibles con **JSON Schema**, que permite restringir:

- Tipos de datos (`bsonType`)
- Campos requeridos
- Expresiones regulares
- Valores mínimos y máximos
- Longitudes
- Validaciones condicionales

### Sintaxis General

```javascript
db.createCollection("nombre", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["campo1", "campo2"],
      properties: {
        campo1: { bsonType: "string" },
        campo2: { bsonType: "int", minimum: 0 }
      }
    }
  },
  validationLevel: "strict", // o "moderate"
  validationAction: "error"  // o "warn"
})
```

------

## 2. Nivel 1 – Validación básica por tipo de dato

### Ejemplo: Validar una colección de usuarios

```
db.createCollection("usuarios", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["nombre", "edad"],
      properties: {
        nombre: { bsonType: "string" },
        edad: { bsonType: "int" }
      }
    }
  }
});
```

### Inserción válida

```
db.usuarios.insertOne({ nombre: "Pedro", edad: 28 });
```

### Inserción inválida (edad string)

```
db.usuarios.insertOne({ nombre: "Ana", edad: "veinte" }); // Rechazado
```

------

## 3. Nivel 2 – Validación con restricciones

### Ejemplo: Validar longitud y rango

```
db.createCollection("clientes", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["nombre", "correo", "edad"],
      properties: {
        nombre: {
          bsonType: "string",
          minLength: 3,
          maxLength: 50
        },
        correo: {
          bsonType: "string",
          pattern: "^.+@.+\\..+$"
        },
        edad: {
          bsonType: "int",
          minimum: 18,
          maximum: 65
        }
      }
    }
  }
});
```

------

## 4. Nivel 3 – Validación condicional y con expresiones (No compatible con MongoDB - Razón: Solo es compatible con el estándar 7 de JSON)

### Ejemplo: Validar un campo solo si otro existe

```
db.createCollection("empleados", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["nombre", "tipo"],
      properties: {
        nombre: { bsonType: "string" },
        tipo: { enum: ["planta", "temporal"] },
        contrato: {
          bsonType: "string"
        }
      },
      if: { properties: { tipo: { const: "temporal" } } },
      then: { required: ["contrato"] }
    }
  }
});
```

------

## 5. Nivel 4 – Uso combinado de validaciones

### Ejemplo: Producto con múltiples restricciones

```
db.createCollection("productos", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["nombre", "precio", "categorias"],
      properties: {
        nombre: {
          bsonType: "string",
          minLength: 3
        },
        precio: {
          bsonType: "double",
          minimum: 0
        },
        categorias: {
          bsonType: "array",
          items: {
            bsonType: "string"
          },
          minItems: 1
        }
      }
    }
  }
});
```

------

## 6. Nivel 5 – Validación de documentos embebidos y arreglos

### Ejemplo: Orden con detalle embebido

```
db.createCollection("ordenes", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["cliente", "items"],
      properties: {
        cliente: {
          bsonType: "object",
          required: ["nombre", "id"],
          properties: {
            nombre: { bsonType: "string" },
            id: { bsonType: "int" }
          }
        },
        items: {
          bsonType: "array",
          items: {
            bsonType: "object",
            required: ["producto", "cantidad"],
            properties: {
              producto: { bsonType: "string" },
              cantidad: { bsonType: "int", minimum: 1 }
            }
          }
        }
      }
    }
  }
});
```

------

## 🧩 Taller Final: Ejercicios

Crea las siguientes colecciones y define las reglas de validación correspondientes:

1. **estudiantes**:
   - Campos: `nombre` (string), `codigo` (string, patrón alfanumérico de 6 caracteres), `promedio` (double, entre 0 y 5).
   - Requiere que el promedio sea obligatorio solo si está en estado "activo".
2. **vehiculos**:
   - Campos: `placa` (string, 6 caracteres), `tipo` (enum: ["carro", "moto"]), `cilindraje` (requerido si tipo es "moto").
3. **eventos**:
   - Campos: `titulo`, `fecha_inicio`, `fecha_fin`.
   - Validar que `fecha_fin` no puede ser menor que `fecha_inicio`.
4. **facturas**:
   - Campos: `cliente` (objeto con nombre y NIT), `detalle` (array de productos), `total`.
   - Cada producto debe tener `nombre`, `precio_unitario`, y `cantidad`.
5. **usuariosSistema**:
   - Campos: `username`, `password`, `rol`.
   - Validar que si el rol es "admin", el username debe iniciar con `admin_`.

   # 🧩 Solución – Taller Final: Validación de Datos en MongoDB

## 1. Colección `estudiantes`

```javascript
db.createCollection("estudiantes", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["nombre", "codigo", "estado"],
      properties: {
        nombre: { bsonType: "string" },
        codigo: {
          bsonType: "string",
          pattern: "^[a-zA-Z0-9]{6}$"
        },
        promedio: {
          bsonType: "double",
          minimum: 0,
          maximum: 5
        },
        estado: {
          enum: ["activo", "inactivo"]
        }
      },
      if: {
        properties: { estado: { const: "activo" } }
      },
      then: {
        required: ["promedio"]
      }
    }
  }
});
```

---

## 2. Colección `vehiculos`

```javascript
db.createCollection("vehiculos", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["placa", "tipo"],
      properties: {
        placa: {
          bsonType: "string",
          minLength: 6,
          maxLength: 6
        },
        tipo: {
          enum: ["carro", "moto"]
        },
        cilindraje: {
          bsonType: "int",
          minimum: 50
        }
      },
      if: {
        properties: { tipo: { const: "moto" } }
      },
      then: {
        required: ["cilindraje"]
      }
    }
  }
});
```

---

## 3. Colección `eventos`

> ⚠️ Nota: La validación que compara `fecha_inicio` y `fecha_fin` debe implementarse a nivel de aplicación, ya que JSON Schema en MongoDB no permite comparación directa entre campos.

```javascript
db.createCollection("eventos", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["titulo", "fecha_inicio", "fecha_fin"],
      properties: {
        titulo: { bsonType: "string" },
        fecha_inicio: { bsonType: "date" },
        fecha_fin: { bsonType: "date" }
      }
    }
  }
});
```

---

## 4. Colección `facturas`

```javascript
db.createCollection("facturas", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["cliente", "detalle", "total"],
      properties: {
        cliente: {
          bsonType: "object",
          required: ["nombre", "NIT"],
          properties: {
            nombre: { bsonType: "string" },
            NIT: { bsonType: "string" }
          }
        },
        detalle: {
          bsonType: "array",
          items: {
            bsonType: "object",
            required: ["nombre", "precio_unitario", "cantidad"],
            properties: {
              nombre: { bsonType: "string" },
              precio_unitario: { bsonType: "double", minimum: 0 },
              cantidad: { bsonType: "int", minimum: 1 }
            }
          }
        },
        total: { bsonType: "double", minimum: 0 }
      }
    }
  }
});
```

---

## 5. Colección `usuariosSistema`

```javascript
db.createCollection("usuariosSistema", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["username", "password", "rol"],
      properties: {
        username: { bsonType: "string" },
        password: { bsonType: "string", minLength: 6 },
        rol: { enum: ["admin", "user"] }
      },
      if: {
        properties: { rol: { const: "admin" } }
      },
      then: {
        properties: {
          username: {
            pattern: "^admin_"
          }
        }
      }
    }
  }
});
```