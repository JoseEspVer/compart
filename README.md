[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/j30MQZnm)
# 📋 Tarea Semanal: Proyecto E2E - Entrega 1

## Descripción 💡
Esta tarea implica la implementación inicial del proyecto E2E. Deben desarrollar las **entidades** correspondientes a la base de datos, la capa de **lógica de negocios** a través de los **servicios**, y la capa de **controladores** para manejar las peticiones HTTP y sus respuestas.


## Indicaciones

**IMPORTANTE** 🚨

- No se debe mover ni cambiar el nombre ni la ubicación de ningún archivo, ya que esto podría ocasionar fallas en la calificación automática y resultar en una puntuación de 0.
- Se especificó el uso de `enum`s para ciertos atributos en algunas clases. A continuación se detallan estos `enum`s, cuya correcta implementación es crucial para evitar posibles fallos en las pruebas automatizadas.

| Nombre del `enum` | Es atributo de ... | Valores                                                |
|-------------------|--------------------|--------------------------------------------------------|
| Category          | `Driver`           | X, XL, BLACK                                           |
| Status            | `Ride`             | REQUESTED, ACCEPTED, IN_PROGRESS, COMPLETED, CANCELLED |
| Role              | `User`             | ADMIN, PASSENGER, DRIVER                               |

- La tabla intermedia `UserLocation`, que relaciona `Passenger` y `Coordinate`, utiliza una clave primaria compuesta, lo que significa que tiene dos claves primarias. JPA no puede implementar esto mediante anotaciones directas, por lo que se debe seguir el enfoque detallado en este [enlace](https://www.codejava.net/frameworks/spring/spring-data-jpa-composite-primary-key-examples). Esta peculiaridad técnica permite manejar tablas intermedias con atributos adicionales, mientras se aprende a sortear esta limitación de JPA.
- Aunque el uso de tablas intermedias con atributos adicionales no es inusual ni problemático a nivel de base de datos, puede causar un problema específico al serializar a JSON debido a la generación de referencias circulares. Para evitar este problema, utilicen la siguiente anotación en las clases de entidades mencionadas anteriormente: `@JsonIdentityInfo(generator = ObjectIdGenerators.PropertyGenerator.class, property = "id")`.

### Entidades ⚙️ 

Las entidades requeridas fueron presentadas al final de la primera semana. Las entidades adicionales agregadas por los alumnos no serán consideradas en la calificación automática, pero se permite su inclusión si es necesario, siempre y cuando se discuta con los profesores.

### Endpoints 🌐

A continuación se detallan los endpoints que deben implementar por entidad y sus descripciones:

> Los `Query Parameters` deben de tener el mismo nombre que se indica en estas tablas. 

#### Recurso `Driver`

| Método HTTP | URI                   | Path Parameter | Query Parameter     | Request Body | Http Status    | Response Body |
|-------------|-----------------------|----------------|---------------------|--------------|----------------|---------------|
| GET         | /driver/{id}          | id             | -                   |              | 200 OK         | Driver        |
| POST        | /driver               |                | -                   | `Driver`     | 201 Created    |               |
| DELETE      | /driver/{id}          | id             | -                   |              | 204 No Content |               |
| PUT         | /driver/{id}          | id             | -                   | `Driver`     | 200 OK         |               |
| PATCH       | /driver/{id}/location | id             | latitude, longitude |              | 200 OK         |               |
| PATCH       | /driver/{id}/car      | id             | -                   | `Vehicle`    | 200 OK         |               |

#### Recurso `Passenger`

| Método HTTP | URI                                   | Path Parameter   | Query Parameter | Request Body | Http Status    | Response Body        |
|-------------|---------------------------------------|------------------|-----------------|--------------|----------------|----------------------|
| GET         | /passenger/{id}                       | id               | -               |              | 200 OK         | `Passenger`          |
| DELETE      | /passenger/{id}                       | id               | -               |              | 204 No Content |                      |
| PATCH       | /passenger/{id}                       | id               | description     | `Coordinate` | 200 OK         |                      |
| GET         | /passenger/{id}/places                | id               | -               |              | 200 OK         | `List\<Coordinate\>` |
| DELETE      | /passenger/{id}/places/{coordinateId} | id, coordinateId | -               |              | 204 No Content |                      |


#### Recurso `Review`

| Método HTTP | URI          | Path Parameter | Query Parameter | Request Body | Http Status | Response Body |
|-------------|--------------|----------------|-----------------|--------------|-------------|---------------|
| POST        | /review      | -              | rideId          | `Review`     | 200 (OK)    |               |
| DELETE      | /review/{id} | id (Long)      | -               | -            | 200 (OK)    |               |


#### Recurso `Ride`

| Método HTTP | URI                   | Path Parameter | Query Parameter | Request Body | Http Status    | Response Body  |
|-------------|-----------------------|----------------|-----------------|--------------|----------------|----------------|
| POST        | /ride                 | -              | -               | `Ride`       | 200 OK         |                |
| PATCH       | /ride/assign/{rideId} | rideId         | -               | -            | 200 OK         |                |
| DELETE      | /ride/{rideId}        | rideId         | -               | -            | 204 No Content |                |
| GET         | /ride/{userId}        | userId         | page, size      | -            | 200 OK         | `Page\<Ride\>` |

**Importante** 🚨: Los códigos de estado mostrados son para casos de éxito. No se restrinjan a estos, ya que también se evaluarán los códigos de estado para casos de error.

## 🛠️ Cómo ejecutar las pruebas localmente 🛠️

Si deseas ejecutar localmente los test para conocer tu avance usa el script ``runtest`` ubicado en la raiz del proyecto.

```bash
./runtest [NombreDelTest]
```

Puedes encontrar los test en las subcarpetas de la carpeta `src/test/java/`

### ⚠️ Al ejecutar los test tener en cuenta:

- Debes tener el archivo `mvnw` que autogenera Spring Initializr en la raíz del proyecto.
- Los test que contienen las palabra **Integration** testean la aplicación completa, se recomienda ejecutarlos al final.
- Los test que contienen la palabra  **Repository** testean la capa de persistencia junto con las entidades y usan Docker, asegurate que se esté ejecutando.
- Los test que tiene un nombre como  **EntidadTest** testean la entidad correspondiente y su relación con otras entidades.

¡Buena suerte! 🚀