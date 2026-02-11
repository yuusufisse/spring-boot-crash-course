# Spring Boot Crash Course

A comprehensive REST API application built with Spring Boot and Kotlin, demonstrating modern backend development practices including authentication, authorization, and CRUD operations.

## 🚀 Features

- **User Authentication & Authorization**
  - JWT (JSON Web Token) based authentication
  - Refresh token mechanism
  - Secure password hashing
  - Email validation
  - Password strength validation

- **Note Management**
  - Create, Read, Update, and Delete (CRUD) operations
  - User-specific note ownership
  - Color-coded notes
  - Timestamp tracking

- **Security**
  - Spring Security integration
  - JWT-based stateless authentication
  - Role-based access control
  - Secure password storage

## 🛠️ Technology Stack

- **Backend Framework:** Spring Boot 3.4.9
- **Language:** Kotlin 1.9.25
- **Database:** MongoDB (Reactive)
- **Security:** Spring Security + JWT
- **Build Tool:** Gradle (Kotlin DSL)
- **Java Version:** 17
- **Containerization:** Docker

### Dependencies

- Spring Boot Starter Web
- Spring Boot Starter Data MongoDB (Reactive)
- Spring Boot Starter Security
- Spring Boot Starter Validation
- JWT (jjwt) 0.12.6
- Kotlin Coroutines & Reactor

## 📋 Prerequisites

Before running this application, make sure you have:

- Java 17 or higher
- MongoDB instance (local or remote)
- Docker (optional, for containerized deployment)

## 🔧 Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yuusufisse/spring-boot-crash-course.git
   cd spring-boot-crash-course
   ```

2. **Build the project**
   ```bash
   ./gradlew build
   ```

## ⚙️ Configuration

### Environment Variables

Set the following environment variables before running the application:

- `MONGODB_CONNECTION_STRING`: Your MongoDB connection string
  ```
  mongodb://username:password@localhost:27017/database_name
  ```

- `JWT_SECRET_BASE64`: Base64 encoded secret key for JWT signing
  ```bash
  # Generate a secure secret:
  openssl rand -base64 32
  ```

### Application Properties

The application runs on port `8085` by default. You can modify this in `src/main/resources/application.properties`.

## 🚀 Running the Application

### Local Development

```bash
# Set environment variables
export MONGODB_CONNECTION_STRING="mongodb://localhost:27017/spring_boot_crash_course"
export JWT_SECRET_BASE64="your-base64-encoded-secret"

# Run the application
./gradlew bootRun
```

### Docker Deployment

1. **Build the Docker image**
   ```bash
   ./gradlew bootBuildImage
   # Or build with a specific name
   docker build -t spring-boot-crash-course .
   ```

2. **Configure environment variables**
   Create a `.env` file:
   ```env
   APP_IMAGE=your-image-name:latest
   APP_PORT=8085
   MONGODB_CONNECTION_STRING=mongodb://your-mongodb-uri
   JWT_SECRET_BASE64=your-base64-secret
   ```

3. **Run with Docker Compose**
   ```bash
   docker-compose up -d
   ```

## 📚 API Endpoints

### Authentication Endpoints

#### Register a new user
```http
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecurePass123"
}
```

**Password Requirements:**
- At least 9 characters long
- Contains at least one uppercase letter
- Contains at least one lowercase letter
- Contains at least one digit

#### Login
```http
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecurePass123"
}
```

**Response:**
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIs...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

#### Refresh Token
```http
POST /auth/refresh
Content-Type: application/json

{
  "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

### Note Endpoints

All note endpoints require authentication. Include the JWT token in the Authorization header:
```
Authorization: Bearer {accessToken}
```

#### Create or Update a Note
```http
POST /notes
Authorization: Bearer {accessToken}
Content-Type: application/json

{
  "id": "optional-note-id-for-update",
  "title": "My Note",
  "content": "Note content here",
  "color": 4294967295
}
```

#### Get All Notes for Current User
```http
GET /notes
Authorization: Bearer {accessToken}
```

#### Get a Specific Note
```http
GET /notes/{id}
Authorization: Bearer {accessToken}
```

#### Update a Note
```http
PUT /notes/{id}
Authorization: Bearer {accessToken}
Content-Type: application/json

{
  "title": "Updated Title",
  "content": "Updated content",
  "color": 4294967295
}
```

#### Delete a Note
```http
DELETE /notes/{id}
Authorization: Bearer {accessToken}
```

## 🧪 Testing

Run the test suite:

```bash
./gradlew test
```

## 📁 Project Structure

```
src/main/kotlin/com/yusuf/spring_boot_crash_course/
├── SpringBootCrashCourseApplication.kt  # Main application entry point
├── GlobalValidationHandler.kt            # Global validation error handler
├── controller/
│   ├── AuthController.kt                 # Authentication endpoints
│   └── NoteController.kt                 # Note CRUD endpoints
├── security/
│   ├── SecurityConfig.kt                 # Security configuration
│   ├── JwtService.kt                     # JWT token generation/validation
│   ├── JwtAuthFilter.kt                  # JWT authentication filter
│   ├── AuthService.kt                    # Authentication service
│   └── HashEncoder.kt                    # Password hashing
└── database/
    ├── model/
    │   ├── User.kt                       # User entity
    │   ├── Note.kt                       # Note entity
    │   └── RefreshToken.kt               # Refresh token entity
    └── repository/
        ├── UserRepository.kt             # User data access
        ├── NoteRepository.kt             # Note data access
        └── RefreshTokenRepository.kt     # Refresh token data access
```

## 🔒 Security Considerations

- All passwords are hashed using Spring Security's BCrypt
- JWT tokens are used for stateless authentication
- Refresh tokens are stored in MongoDB for token rotation
- API endpoints are protected with Spring Security
- Input validation is enforced on all requests

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📝 License

This project is intended for educational purposes as part of a Spring Boot crash course.

## 👨‍💻 Author

Yusuf Isse - [yuusufisse](https://github.com/yuusufisse)
