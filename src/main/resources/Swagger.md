# 4. Documentacion OpenAPI / Swagger

## 1. Configuracion de springdoc-openapi

La dependencia utilizada es:

```
<dependency>
	<groupId>org.springdoc</groupId>
	<artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
	<version>2.6.0</version>
</dependency>
```

Esta dependencia integra OpenAPI 3 con Spring Boot y genera la especificacion de la API a partir de los controladores, sus rutas, parametros y anotaciones.

No fue necesario crear una pagina HTML manualmente.

## 2. Informacion general de la API

La clase `OpenApiConfig` define la informacion que aparece en Swagger:

```
@Bean
public OpenAPI api() {
	return new OpenAPI().info(new Info()
			.title("ARSW Blueprints API")
			.version("v1")
			.description("Blueprints Laboratory (Java 21 / Spring Boot 3.3.x)"));
}
```

Con esta configuracion, Swagger identifica el nombre, la version y la descripcion del servicio documentado.

## 3. Anotaciones de los endpoints

El controlador `BlueprintsAPIController` tiene una anotacion `@Operation`
para describir la finalidad de cada operacion y una o mas anotaciones
`@ApiResponse` para indicar los posibles codigos HTTP:

| Metodo | Ruta | Descripcion | Respuestas |
| --- | --- | --- | --- |
| GET | `/api/v1/blueprints` | Listar todos los blueprints | `200` |
| GET | `/api/v1/blueprints/{author}` | Listar blueprints de un autor | `200`, `404` |
| GET | `/api/v1/blueprints/{author}/{bpname}` | Consultar un blueprint | `200`, `404` |
| POST | `/api/v1/blueprints` | Crear un blueprint | `201`, `400` |
| PUT | `/api/v1/blueprints/{author}/{bpname}/points` | Agregar un punto | `202`, `404` |

Ejemplo utilizado en el controlador:

```
@Operation(summary = "Consultar un blueprint")
@ApiResponses({
	@ApiResponse(responseCode = "200", description = "Consulta exitosa"),
	@ApiResponse(responseCode = "404", description = "Blueprint inexistente")
})
```

Las anotaciones no cambian la logica de negocio ni la persistencia en PostgreSQL; solamente agregan metadatos para generar la documentacion.

## 4. URLs de documentacion

Con la aplicacion ejecutandose, se puede acceder a:

- Swagger UI: http://localhost:8080/swagger-ui.html
- Especificacion OpenAPI en JSON: http://localhost:8080/v3/api-docs

Swagger UI permite consultar cada endpoint, revisar sus parametros y ejecutar peticiones desde el navegador.

## 5. Verificacion con Docker y PostgreSQL

Para validar la integracion completa, primero se inicia el contenedor de PostgreSQL y luego la aplicacion:

```
mvn test
mvn spring-boot:run
```

Despues se abre Swagger UI y se ejecutan los endpoints con los datos de prueba. La respuesta debe conservar el formato uniforme de `ApiResponse` y los codigos HTTP definidos por la API.

**Pruebas**:

GET `/api/v1/blueprints`:

![alt text](/src/main/resources/images/swagger0.png)

---

GET  `/api/v1/blueprints/{author}`:

valido:

![alt text](/src/main/resources/images/swagger1.png)

invalido:

![alt text](/src/main/resources/images/swagger2.png)

---

GET `/api/v1/blueprints/{author}/{bpname}`

valido:

![alt text](/src/main/resources/images/swagger3.png)

invalido:

![alt text](/src/main/resources/images/swagger4.png)

---

POST `/api/v1/blueprints`

![alt text](/src/main/resources/images/swagger5.png)

---

PUT `/api/v1/blueprints/{author}/{bpname}/points`

![alt text](/src/main/resources/images/swagger6.png)

---

![alt text](/src/main/resources/images/swagger7.png)