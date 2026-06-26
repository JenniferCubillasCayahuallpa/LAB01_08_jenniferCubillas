# LAB01_08_jenniferCubillas

## Descripción
Proyecto de microservicios con Spring Boot y PostgreSQL.  
Compuesto por dos servicios independientes que se comunican vía REST.

## Arquitectura

## Microservicios

### ms-productos (puerto 8081)
Gestiona el catálogo de productos.

| Método | Endpoint                        | Descripción                  |
|--------|---------------------------------|------------------------------|
| GET    | /api/productos                  | Lista productos activos       |
| GET    | /api/productos/todos            | Lista todos los productos     |
| GET    | /api/productos/{id}             | Busca producto por ID        |
| POST   | /api/productos                  | Crea un producto             |
| PUT    | /api/productos/{id}             | Actualiza un producto        |
| DELETE | /api/productos/{id}             | Desactiva un producto        |
| PUT    | /api/productos/{id}/stock       | Reduce stock (usado por ms-pedidos) |

### ms-pedidos (puerto 8082)
Gestiona los pedidos, consultando productos en ms-productos.

| Método | Endpoint                          | Descripción                  |
|--------|-----------------------------------|------------------------------|
| GET    | /api/pedidos                      | Lista todos los pedidos      |
| GET    | /api/pedidos/{id}                 | Busca pedido por ID          |
| POST   | /api/pedidos                      | Crea un pedido               |
| PATCH  | /api/pedidos/{id}/estado          | Actualiza estado del pedido  |
| GET    | /api/pedidos/producto/{productId} | Pedidos por producto         |

---

## Requisitos previos
- Java 17+
- Maven 3.8+
- PostgreSQL 14+

---

## Configuración de Base de Datos

Crear las dos bases de datos en PostgreSQL:

```sql
CREATE DATABASE db_productos;
CREATE DATABASE db_pedidos;
```

## Cómo ejecutar

### ms-productos
```bash
cd ms-productos
mvn spring-boot:run
```

### ms-pedidos (en otra terminal)
```bash
cd ms-pedidos
mvn spring-boot:run

### Crear un pedido
```bash
curl -X POST http://localhost:8082/api/pedidos \
  -H "Content-Type: application/json" \
  -d '{"productId": 1, "quantity": 2}'
```

### Listar productos
```bash
curl http://localhost:8081/api/productos
```

### Actualizar estado de pedido
```bash
curl -X PATCH http://localhost:8082/api/pedidos/1/estado \
  -H "Content-Type: application/json" \
  -d '{"status": "COMPLETED"}'
