# Sistema de Gestión de Reservas de Hotel

Sistema web para la gestión de reservas de habitaciones de hotel. Permite el registro e inicio de sesión de usuarios (con roles de **Administrador** y **Cliente**), la búsqueda de habitaciones disponibles por rango de fechas, y la creación, consulta, modificación y eliminación de reservaciones.

El proyecto FullStack, backend (API REST), frontend (interfaz web) y documentación UML del diseño del sistema.

## Tecnologías utilizadas

**Backend**
- Java 11
- Spring Boot 2.5.4 (Spring Web, Spring Data JPA)
- Base de datos H2 (embebida, modo servidor)
- Maven

**Frontend**
- HTML5, CSS3, JavaScript
- Axios (para las peticiones HTTP a la API)

## Estructura del proyecto

```
ReservacionesHotel-main/
├── SpringBoot/                     # Backend (API REST)
│   └── src/main/java/com/miempresa/springboot/
│       ├── controller/              # Endpoints REST (Usuario, Habitacion, Reserva)
│       ├── model/                   # Entidades JPA (Usuario, Habitacion, Reserva, Role)
│       ├── repository/              # Repositorios Spring Data JPA
│       ├── config/                  # Configuración de CORS (WebConfig)
│       ├── exception/                # Manejo de excepciones (ResourceNotFoundException)
│       └── Application.java         # Clase principal de arranque
├── INICIO DE SESION/                # Frontend (HTML, CSS, JS)
│   ├── Inicio_Sesion.html           # Login y registro
│   ├── Inicio.html                  # Vista de cliente
│   ├── Inicio_Admin.html            # Vista de administrador
│   └── assets/                      # CSS, JS e imágenes
├── UML/                             # Diagramas de clases, casos de uso y secuencia (PDF)
├── PROYECTO.docx                    # Documentación del proyecto
└── Sistema de Gestión de Reservas de Hotel.pptx   # Presentación del proyecto
```

## Modelo de datos

- **Usuario**: id, nombre, email, password, role (`ADMIN` o `CLIENTE`).
- **Habitacion**: id, tipo, precio, disponible (calculado, no persistido).
- **Reserva**: id, usuario, habitacion, fechaInicio, fechaFin.

## Endpoints principales de la API

Base URL local: `http://localhost:8080`

### Usuarios
| Método | Endpoint | Descripción |
|---|---|---|
| GET | `/usuarios` | Lista todos los usuarios |
| GET | `/usuarios/{id}` | Obtiene un usuario por id |
| POST | `/usuarios/registro` | Registra un nuevo usuario (rol `CLIENTE` por defecto) |
| POST | `/usuarios/login` | Inicia sesión con email y password |
| PUT | `/usuarios/{id}` | Actualiza un usuario |
| DELETE | `/usuarios/{id}` | Elimina un usuario |

### Habitaciones
| Método | Endpoint | Descripción |
|---|---|---|
| GET | `/habitaciones` | Lista todas las habitaciones |
| GET | `/habitaciones/{id}` | Obtiene una habitación por id |
| GET | `/habitaciones/disponibilidad?fechaInicio=&fechaFin=` | Devuelve habitaciones marcando su disponibilidad para el rango dado |
| POST | `/habitaciones` | Crea una habitación |
| PUT | `/habitaciones/{id}` | Actualiza una habitación |
| DELETE | `/habitaciones/{id}` | Elimina una habitación |

### Reservas
| Método | Endpoint | Descripción |
|---|---|---|
| GET | `/reservas` | Lista todas las reservas |
| GET | `/reservas/{id}` | Obtiene una reserva por id |
| POST | `/reservas` | Crea una reserva (requiere usuario, habitación y fechas) |
| PUT | `/reservas/{id}` | Actualiza una reserva |
| DELETE | `/reservas/{id}` | Elimina una reserva |

> Nota: las respuestas de error usan `ResourceNotFoundException`, que devuelve un HTTP `404` cuando el recurso solicitado no existe.

## Instalación y ejecución

### Requisitos previos
- JDK 11 o superior
- Maven (o usar el wrapper `mvnw` incluido)
- Un navegador web
- (Opcional) Live Server u otro servidor estático para servir el frontend, ya que el backend tiene CORS configurado para `http://127.0.0.1:5500`

### 1. Backend

```bash
cd SpringBoot
./mvnw spring-boot:run
```

En Windows:

```bash
cd SpringBoot
mvnw.cmd spring-boot:run
```

El servidor arrancará en `http://localhost:8080`.

La consola de H2 queda disponible en `http://localhost:8080/h2-console` con la siguiente configuración (definida en `application.properties`):

- JDBC URL: `jdbc:h2:~/testdb;AUTO_SERVER=TRUE`
- Usuario: `proyectoFinal`
- Password: `1234`

### 2. Frontend

Los archivos están en `INICIO DE SESION/`. Se recomienda servirlos con Live Server (extensión de VS Code) en el puerto `5500`, ya que el backend solo permite peticiones CORS desde `http://127.0.0.1:5500`.

1. Abrir la carpeta `INICIO DE SESION` en VS Code.
2. Ejecutar `Inicio_Sesion.html` con Live Server.
3. Registrarse o iniciar sesión. Según el rol del usuario, se redirige a `Inicio.html` (cliente) o `Inicio_Admin.html` (administrador).

> Si se sirve el frontend desde otro puerto o dominio, es necesario actualizar `allowedOrigins` en `SpringBoot/src/main/java/com/miempresa/springboot/config/WebConfig.java`.

## Documentación adicional

- `UML/`: diagrama de clases, diagrama de casos de uso y diagrama de secuencia del sistema.
- `PROYECTO.docx`: documento con el detalle del proyecto.
- `Sistema de Gestión de Reservas de Hotel.pptx`: presentación del proyecto.
