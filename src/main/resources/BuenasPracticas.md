# Buenas Prácticas API REST y Evidencias

## Record Genérico

La idea implementada es que toda respuesta generada por la API tenga una misma forma, sin importar si es un Blueprint, lista de Blueprints o nada.
Con esto en mente, cuando el cliente consume la API siempre sabrá donde buscar el código, el mensaje y los datos, en vez de tener estructuras distintas por endpoints separados, para ello se definió con el genérico ```<T>```.

Con esto, solo un record se encarga de cubrir todos los casos posibles:
```
ApiResponse<Set<Blueprints>>
ApiResponse<Blueprints>
ApiResponse<Void>
```

## Cambio de path

```
/api/v1/blueprints
```
El cambio del PATH es convención de REST.
* El **/api** deja claro que es una API
* El **/v1** versiona el contrato
* El **/blueprints** es requerimiento del laboratorio

## Código HTTP

Hacemos uso de códigos HTTP específicos según cada retorno de consulta para identificar correctamente cada uno:

| Endpoint | Código | Por qué |
| -------- | ------ | ------- |
| GET | 200 | Consulta que obtuvo un exito, es un estandar de lecturas correctas |
| POST | 201 | Funcionó y se creó un nuevo recurso |
| PUT | 202 | Se aceptó la petición y se diferencia de los 2 códigos ateriores |
| No encontrado | 404 | El recurso solicitado no existe |
| Inválido | 400 | El error viene de parte del cliente |

## Evidencias

### Nuevos curl

```
curl -s http://localhost:8080/api/v1/blueprints | jq
curl -s http://localhost:8080/api/v1/blueprints/john | jq
curl -s http://localhost:8080/api/v1/blueprints/john/house | jq
curl -i -X POST http://localhost:8080/api/v1/blueprints -H 'Content-Type: application/json' -d '{ "author":"john","name":"kitchen","points":[{"x":1,"y":1},{"x":2,"y":2}] }'
curl -i -X PUT  http://localhost:8080/api/v1/blueprints/john/kitchen/points -H 'Content-Type: application/json' -d '{ "x":3,"y":3 }'
```

Con este cambio de PATH vemos los resultados y ahora tenemos un mensaje claro después de la ejecución de cada uno.

### Los 3 GET

**TODOS LOS DATOS**

![get1](/src/main/resources/images/get1.png)

**POR AUTOR**

![get2](/src/main/resources/images/get2.png)

**POR AUTOR Y NOMBRE**

![get2](/src/main/resources/images/get3.png)

**DATOS INICIALES EN LA BD**

![init](/src/main/resources/images/initial.png)

---

### POST

**UN NUEVO NOBRE CON NUEVOS PUNTOS**

![post](/src/main/resources/images/post1.png)

**ACTUALIZACIÓN EL LA BD**

![post1](/src/main/resources/images/post.png)

---

### PUT

**UN NUEVO PUNTO A UN AUTOR Y NOMBRE EXISTENTE**

![put](/src/main/resources/images/put1.png)

**ACTUALIZACIÓN EN LA BD**

![put](/src/main/resources/images/put.png) 