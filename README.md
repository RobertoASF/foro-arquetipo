# foro-arquetipo

Arquetipo Maven para crear los microservicios del foro de TCG con la misma estructura, configuración y reglas de seguridad. Sirve como plantilla para el equipo: en vez de copiar y pegar un proyecto, se genera uno nuevo ya listo para conectarse a Oracle Cloud y validar el token de `ms-usuarios`.

| Dato | Valor |
|---|---|
| groupId | `cl.duoc.foro` |
| artifactId | `foro-microservicio-archetype` |
| Versión | `1.0.1` |

## Qué trae el proyecto generado

```
ms-<dominio>/
├── Dockerfile                  Compilación en dos etapas y usuario sin privilegios
├── pom.xml                     Spring Boot 4.1, Java 21, JPA, Security, Oracle con Wallet
├── db/01_tablas.sql            Script de tablas del esquema propio
└── src
    ├── main/java/cl/duoc/foro/<dominio>
    │   ├── config       SeguridadConfig: sesión sin estado y validación del JWT
    │   ├── controller   Endpoints REST
    │   ├── dto          ErrorResponse y MensajeResponse
    │   ├── exception    Excepciones propias y ManejadorErrores (errores en JSON)
    │   ├── model        Entidades JPA
    │   ├── repository   Repositorios de Spring Data
    │   ├── security     UsuarioAutenticado y respuestas 401 y 403 en JSON
    │   └── service      Reglas de negocio
    ├── main/resources/application.properties
    └── test/java        Pruebas de seguridad y del usuario del token
```

Todos los microservicios del foro siguen esta misma organización por capas:

- **Controller**: recibe la petición, valida el DTO y responde JSON.
- **Service**: aplica las reglas de negocio y los permisos.
- **Repository**: lee y guarda en Oracle con Spring Data JPA.
- **DTO**: separa lo que entra y sale de la API de las entidades.

## Propiedades

| Propiedad | Obligatoria | Ejemplo | Uso |
|---|---|---|---|
| `artifactId` | Sí | `ms-moderacion` | Nombre del proyecto |
| `dominio` | Sí | `moderacion` | Paquete y esquema por defecto |
| `nombreClase` | Sí | `MsModeracion` | Clase principal `MsModeracionApplication` |
| `puerto` | Sí | `8083` | Puerto del servicio y del Dockerfile |
| `descripcion` | No | `Microservicio de moderación` | README y pom |
| `esquema` | No | `foro_moderacion` | Usuario de Oracle |
| `package` | No | `cl.duoc.foro.moderacion` | Paquete base |

## Instalarlo

```bash
mvn install
```

Esto deja el arquetipo en el repositorio local de Maven. Durante la instalación se ejecuta una prueba que genera un proyecto de ejemplo y corre sus pruebas, así se comprueba que la plantilla compila.

## Crear un microservicio

```bash
mvn archetype:generate \
  -DarchetypeGroupId=cl.duoc.foro \
  -DarchetypeArtifactId=foro-microservicio-archetype \
  -DarchetypeVersion=1.0.1 \
  -DgroupId=cl.duoc.foro \
  -DartifactId=ms-moderacion \
  -Ddominio=moderacion \
  -DnombreClase=MsModeracion \
  -Dpuerto=8083 \
  -Ddescripcion="Microservicio de moderación del foro" \
  -DinteractiveMode=false
```

Así se creó `foro-ms-moderacion`. Después solo falta agregar las entidades, repositorios, servicios y controladores del nuevo dominio.

## Versiones

Se usa versionado semántico. Cada versión queda marcada con un tag `vX.Y.Z` en `main`.

| Versión | Cambios |
|---|---|
| 1.0.1 | La prueba de seguridad ya no carga los controladores del proyecto generado |
| 1.0.0 | Primera versión de la plantilla |
