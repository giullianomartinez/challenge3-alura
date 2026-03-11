# 🗣️ ForoHub — API REST con Spring Boot

API REST para gestión de tópicos de un foro, desarrollada como parte del Challenge del programa **Oracle Next Education (ONE)** junto a **Alura Latam**.

---

## 📋 ¿Qué hace esta aplicación?

Permite a usuarios autenticados crear, consultar, actualizar y eliminar tópicos de discusión, validando que no existan duplicados y protegiendo todos los endpoints con tokens JWT.

---

## 🛠️ Tecnologías utilizadas

| Tecnología | Versión |
|---|---|
| Java JDK | 17+ |
| Spring Boot | 3.2+ |
| Spring Security | Incluido en Boot 3 |
| Spring Data JPA | Incluido en Boot 3 |
| Flyway | Incluido en Boot 3 |
| MySQL | 8+ |
| Lombok | Última estable |
| JWT (auth0) | 4.4.0 |
| Maven | 4+ |

---

## 📁 Estructura del proyecto

```
foroHub/
├── src/main/java/com/alura/foroHub/
│   ├── ForoHubApplication.java       ← Punto de entrada
│   ├── controller/
│   │   ├── AuthController.java       ← POST /login
│   │   └── TopicoController.java     ← CRUD /topicos
│   ├── model/
│   │   ├── Topico.java
│   │   ├── Usuario.java
│   │   └── StatusTopico.java
│   ├── repository/
│   │   ├── TopicoRepository.java
│   │   └── UsuarioRepository.java
│   ├── service/
│   │   ├── TopicoService.java
│   │   └── AutenticacionService.java
│   ├── security/
│   │   ├── TokenService.java
│   │   ├── SecurityFilter.java
│   │   └── SecurityConfigurations.java
│   ├── dto/
│   │   ├── TopicoDTO.java
│   │   └── AuthDTO.java
│   └── infra/
│       └── ManejadorDeErrores.java
├── src/main/resources/
│   ├── application.properties
│   └── db/migration/
│       ├── V1__create-table-topicos.sql
│       └── V2__create-table-usuarios.sql
└── pom.xml
```

---

## ⚙️ Configuración

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/foroHub.git
cd foroHub
```

### 2. Configurar `application.properties`

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/foroHub?createDatabaseIfNotExist=true
spring.datasource.username=root
spring.datasource.password=TU_PASSWORD_AQUI
spring.jpa.show-sql=true
api.security.token.secret=foroHub-secret-key-2024
```

### 3. Insertar un usuario de prueba en la BD

La contraseña debe estar hasheada con BCrypt. Ejecuta esto en MySQL:

```sql
INSERT INTO usuarios (login, clave)
VALUES ('admin@foro.com', '$2a$10$Y6J7Qz1Qqz1Qqz1Qqz1QuOExample...');
```

> Puedes generar un hash BCrypt en: [bcrypt-generator.com](https://bcrypt-generator.com)

### 4. Ejecutar

```bash
mvn spring-boot:run
```

La API quedará disponible en `http://localhost:8080`

---

## 🔗 Endpoints

### Autenticación

| Método | URL | Descripción | Auth |
|---|---|---|---|
| POST | `/login` | Obtener token JWT | ❌ No requiere |

**Body de ejemplo:**
```json
{
  "login": "admin@foro.com",
  "clave": "123456"
}
```

**Respuesta:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9..."
}
```

---

### Tópicos (todos requieren token)

| Método | URL | Descripción |
|---|---|---|
| POST | `/topicos` | Crear tópico |
| GET | `/topicos` | Listar tópicos |
| GET | `/topicos/{id}` | Ver un tópico |
| PUT | `/topicos/{id}` | Actualizar tópico |
| DELETE | `/topicos/{id}` | Eliminar tópico |

Para las solicitudes autenticadas, agrega el header:
```
Authorization: Bearer <tu-token-aquí>
```

**Body para crear un tópico:**
```json
{
  "titulo": "¿Cómo usar Spring Security?",
  "mensaje": "Tengo dudas sobre la configuración...",
  "autor": "Ana García",
  "curso": "Spring Boot 3"
}
```

---

## ✅ Reglas de negocio

- Todos los campos son obligatorios al crear un tópico.
- No se permiten dos tópicos con el mismo título **y** mensaje.
- Solo usuarios con token válido pueden acceder a los endpoints de tópicos.
- Los tokens expiran después de 2 horas.

---

## 👨‍💻 Autor

Desarrollado como parte del **Challenge Oracle Next Education — Alura Latam**.

---

## 📄 Licencia

Distribuido bajo la licencia MIT.
