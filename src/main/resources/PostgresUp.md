# Levantar y Ejecutar el proyecto

##  Requisitos previos

- Tener Docker y Docker Compose instalados
- Tener Java 21 y Maven instalados

## 2. Levantar Base de datos PostgreSQL

Desde la consola, dirigirse a la raíz del laboratorio.
(más específicamente, donde está ubicado [docker-compose](../../../docker-compose.yml))

Ejecutar en la consola
```
docker-compose up -d
```
Esto descargará la imagen de Postgres y levantará nuestro contenedor ```postgres-dev``` con la base de datos ```BluePrints``` ya configurada

Verificar que el contenedor está corriendo
```
docker ps
```
Debe aparecer ```postgres-dev``` con estado **Up**

## 3. Ejecutar la Aplicación

Ejecutar los mismos comandos dados al inicio del repositorio para esta ejecución
```
mvn clean install
```
Y a continuación 
```
mvn spring-boot:run
```
En los logs se debe visualizar la conexión exitosa a Postgres y el mensaje **Started BlueprintsApplication**

## 4. Verificar su funcionamiento

Para verificar el correcto funcionamiento hay varias opciones:

- Consultar las salidas de los endPoints proporcionados al inicio en el [README.md](/README.md)

- Desde la consola, consultar la base de datos directamente con el comando:
    ```
    docker exec -it postgres-dev psql -U MorenoRodriguez -d BluePrints
    ```
    Y después de ello, hacer un Query:
    ```
    SELECT * FROM blueprints;
    ```

## 5. Detener el entorno

Para solo detener el entorno:
```
docker-compose down
```
Para eliminar también los datos guardados:
```
docker-compose down -v
```

## 6. Aclaraciones

- El archivo [application.properties](/src/main/resources/application.properties) ya apunta a la ejecución de la base de datos. Por tanto, no se requiere ningún cambio si se usa el mismo [docker-compose.yml](/docker-compose.yml) ya incluido