# crear database tienda, crear coleccion clientes, insertar datos 
use tienda;

db.clientes.drop();

db.clientes.insertMany([
  { nombre: "Ana", edad: 25, ciudad: "Bogotá", compras: [120000, 80000, 90000] },
  { nombre: "Carlos", edad: 31, ciudad: "Medellín", compras: [50000, 60000] },
  { nombre: "Laura", edad: 22, ciudad: "Cali", compras: [45000, 30000, 75000] },
  { nombre: "Luis", edad: 45, ciudad: "Bogotá", compras: [200000, 150000] },
  { nombre: "Sofía", edad: 35, ciudad: "Barranquilla", compras: [90000, 70000, 85000] },
  { nombre: "Pedro", edad: 29, ciudad: "Cali", compras: [30000, 40000] },
  { nombre: "Marta", edad: 40, ciudad: "Medellín", compras: [110000, 105000, 99000] }
]);

# 1. Mostrar nombre y categoría de cada cliente
function mostrarcliente(cliente) {
print("Cliente:", cliente.nombre);
print("categoria:", cliente.categoria);}

# 2. Filtrar clientes cuya media de compras sea superior a $90.000

## crear funcion promedio 

function calcularPromedio(compras) {
  if (!compras.length) return 0;
  var total = compras.reduce((a, b) => a + b, 0);
  return total / compras.length;
}

## funcion 

function clasificarCliente(cliente) {
  var promedio = calcularPromedio(cliente.compras);
  if (promedio >= 90000) 
  return (cliente)
}

# 3. Crear una función que determine si un cliente es joven (edad < 30) y lo imprima



