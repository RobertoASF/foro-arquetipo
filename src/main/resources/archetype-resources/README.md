#set( $symbol_pound = '#' )
# ${artifactId}

${descripcion}.

Creado con el arquetipo `foro-microservicio-archetype`. Hecho con Java 21, Spring Boot 4.1, Spring Data JPA, Spring Security y Oracle.

${symbol_pound}${symbol_pound} Estructura

```
src/main/java/${package.replace('.', '/')}
├── config       Seguridad y validación del token
├── controller   Endpoints REST
├── dto          Datos de entrada y salida, con sus validaciones
├── exception    Errores propios y manejo global
├── model        Entidades JPA
├── repository   Acceso a Oracle
├── security     Datos del usuario autenticado y respuestas 401 y 403
└── service      Reglas de negocio
```

${symbol_pound}${symbol_pound} Base de datos

Los scripts están en la carpeta `db` y se ejecutan conectado como `${esquema.toUpperCase()}`.

${symbol_pound}${symbol_pound} Configuración

Está en `src/main/resources/application.properties`. El servicio usa el puerto `${puerto}` y lee el Wallet de Oracle Cloud desde `/Wallet_dbfullstack3`. Las claves se entregan con variables de entorno:

| Variable | Descripción |
|---|---|
| `DB_PASSWORD` | Clave del esquema `${esquema.toUpperCase()}` |
| `JWT_SECRETO` | El mismo secreto que usa `ms-usuarios` |
| `CORS_ORIGEN` | Origen web permitido |
| `LOG_NIVEL` | Nivel de log del proyecto, por defecto `INFO` |

${symbol_pound}${symbol_pound} Pruebas

```bash
mvn test
```
