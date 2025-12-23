# 🧺 LaundryMart Backend

<div align="center">

![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.2+-brightgreen.svg)
![Java](https://img.shields.io/badge/Java-17-orange.svg)
![MySQL](https://img.shields.io/badge/MySQL-8.0-blue.svg)
![License](https://img.shields.io/badge/license-MIT-blue.svg)

A robust and secure RESTful API built with Spring Boot 3 for a full-stack laundry management system with role-based access control and JWT authentication.

[Features](#-features) • [Tech Stack](#-tech-stack) • [Getting Started](#-getting-started) • [API Documentation](#-api-documentation) • [Contributing](#-contributing)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Configuration](#configuration)
- [API Documentation](#-api-documentation)
- [Security](#-security)
- [Frontend Integration](#-frontend-integration)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [License](#-license)
- [Contact](#-contact)

---

## 🎯 Overview

LaundryMart Backend is a comprehensive laundry management system API that handles user authentication, order management, and role-based access control. Built with modern Spring Boot practices, it provides a secure and scalable foundation for laundry service applications.

### Key Highlights

- 🔐 **Secure Authentication**: JWT-based stateless authentication
- 👥 **Multi-Role Support**: Admin, Customer, Employee, and Rider roles
- 📦 **Order Management**: Complete order lifecycle tracking
- 🚀 **RESTful API**: Clean and intuitive endpoint design
- 🏗️ **Layered Architecture**: Maintainable and scalable codebase
- 🔄 **CORS Enabled**: Ready for React/Vue frontend integration

---

## ✨ Features

### Authentication & Authorization
- ✅ User registration with email validation
- ✅ Secure login with JWT token generation
- ✅ Role-based access control (RBAC)
- ✅ Password encryption using BCrypt
- ✅ Token refresh mechanism

### User Management
- ✅ Multiple user roles: **Admin**, **Customer**, **Employee**, **Rider**
- ✅ User profile management
- ✅ Admin dashboard for user oversight

### Order Management
- ✅ Create and track laundry orders
- ✅ Assign orders to employees and riders
- ✅ Order status updates
- ✅ Order history tracking

### Security
- ✅ JWT-based stateless sessions
- ✅ Protected endpoints with Spring Security
- ✅ CORS configuration for frontend integration
- ✅ SQL injection prevention via JPA

---

## 🛠️ Tech Stack

| Category | Technology |
|----------|-----------|
| **Framework** | Spring Boot 3.2+ |
| **Language** | Java 17 |
| **Security** | Spring Security, JJWT |
| **Database** | MySQL 8.0 |
| **ORM** | Spring Data JPA, Hibernate |
| **Build Tool** | Maven |
| **Additional** | Lombok, Spring Web |

---

## 📁 Project Structure

```
src/main/java/com/laundrymart/backend
├── BackendApplication.java          # Main Spring Boot application
├── config/
│   ├── SecurityConfig.java          # Security & JWT configuration
│   ├── JwtFilter.java               # JWT validation filter
│   └── JwtUtil.java                 # Token generation & validation utilities
├── controller/
│   ├── AuthController.java          # Authentication endpoints (/register, /login)
│   ├── AdminController.java         # Admin-only endpoints
│   ├── CustomerController.java      # Customer endpoints (orders, profile)
│   ├── EmployeeController.java      # Employee endpoints
│   └── RiderController.java         # Rider endpoints
├── entity/
│   ├── User.java                    # User entity model
│   └── Order.java                   # Order entity model
├── repository/
│   ├── UserRepository.java          # User data access
│   └── OrderRepository.java         # Order data access
├── service/
│   ├── UserService.java             # User business logic
│   └── OrderService.java            # Order business logic
├── dto/
│   ├── LoginRequest.java            # Request DTOs
│   ├── RegisterRequest.java
│   └── OrderRequest.java
└── exception/
    └── GlobalExceptionHandler.java  # Centralized exception handling

src/main/resources/
└── application.properties           # Application configuration
```

---

## 🚀 Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

- **Java 17** or higher ([Download](https://www.oracle.com/java/technologies/downloads/))
- **MySQL 8.0** ([Download](https://dev.mysql.com/downloads/))
- **Maven 3.8+** ([Download](https://maven.apache.org/download.cgi))
- **Git** ([Download](https://git-scm.com/downloads))

### Installation

1. **Clone the repository**

```bash
git clone https://github.com/yourusername/laundymart-backend.git
cd laundymart-backend
```

2. **Create MySQL Database**

```sql
CREATE DATABASE laundrymart;
```

3. **Configure Application Properties**

Edit `src/main/resources/application.properties`:

```properties
# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/laundrymart
spring.datasource.username=root
spring.datasource.password=your_mysql_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA/Hibernate Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect

# JWT Configuration (IMPORTANT: Change this secret!)
jwt.secret=YourVeryLongRandomBase64EncodedSecretKeyHere
jwt.expiration=86400000

# Server Configuration
server.port=8080
```

4. **Generate a Secure JWT Secret**

```bash
# Using OpenSSL (Linux/Mac)
openssl rand -base64 64

# Using Node.js
node -e "console.log(require('crypto').randomBytes(64).toString('base64'))"
```

Copy the generated secret and update `jwt.secret` in `application.properties`.

5. **Build and Run**

```bash
# Build the project
mvn clean install

# Run the application
mvn spring-boot:run
```

The server will start at `http://localhost:8080` 🎉

---

## 📚 API Documentation

### Base URL
```
http://localhost:8080
```

### Public Endpoints

#### Register User
```http
POST /register
Content-Type: application/json

{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "SecurePass123!",
  "role": "CUSTOMER"
}
```

**Response:**
```json
{
  "message": "User registered successfully",
  "userId": 1
}
```

#### Login
```http
POST /login
Content-Type: application/json

{
  "username": "john_doe",
  "password": "SecurePass123!"
}
```

**Response:**
```json
{
  "token": "eyJhbGciOiJIUzUxMiJ9...",
  "type": "Bearer",
  "username": "john_doe",
  "role": "CUSTOMER"
}
```

### Protected Endpoints

> **Note:** All protected endpoints require JWT token in the Authorization header:
> ```
> Authorization: Bearer <your_jwt_token>
> ```

#### Admin Endpoints

##### Get All Users
```http
GET /admin/users
Authorization: Bearer <token>
```

##### Delete User
```http
DELETE /admin/users/{userId}
Authorization: Bearer <token>
```

#### Customer Endpoints

##### Place Order
```http
POST /customer/orders
Authorization: Bearer <token>
Content-Type: application/json

{
  "serviceType": "WASH_AND_FOLD",
  "items": ["Shirts: 5", "Pants: 3"],
  "pickupAddress": "123 Main St",
  "deliveryAddress": "123 Main St",
  "pickupDate": "2024-12-20"
}
```

##### Get My Orders
```http
GET /customer/orders
Authorization: Bearer <token>
```

#### Employee Endpoints

##### Get Assigned Orders
```http
GET /employee/orders
Authorization: Bearer <token>
```

##### Update Order Status
```http
PUT /employee/orders/{orderId}/status
Authorization: Bearer <token>
Content-Type: application/json

{
  "status": "IN_PROGRESS"
}
```

### Response Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |

---

## 🔒 Security

### Authentication Flow

1. User registers or logs in
2. Server validates credentials
3. JWT token is generated and returned
4. Client stores token (localStorage/sessionStorage)
5. Token is sent in Authorization header for protected requests
6. Server validates token and processes request

### Security Features

- **Password Hashing**: BCrypt with salt rounds
- **JWT Signature**: HS512 algorithm
- **Token Expiration**: Configurable (default 24 hours)
- **CORS Protection**: Configured for specific origins
- **SQL Injection Prevention**: Parameterized queries via JPA
- **XSS Protection**: Input validation and sanitization

### Important Security Notes

⚠️ **Before deploying to production:**

1. Generate a strong, unique JWT secret (at least 512 bits)
2. Enable HTTPS/TLS
3. Configure CORS for your production domain only
4. Use environment variables for sensitive data
5. Implement rate limiting
6. Add request logging and monitoring

---

## 🎨 Frontend Integration

This backend is designed to work seamlessly with React/Vue frontends.

### Example: Axios Configuration

```javascript
import axios from 'axios';

const api = axios.create({
  baseURL: 'http://localhost:8080',
  headers: {
    'Content-Type': 'application/json'
  }
});

// Add token to requests
api.interceptors.request.use(
  config => {
    const token = localStorage.getItem('token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  error => Promise.reject(error)
);

export default api;
```

### React Frontend

Check out the companion React frontend: [laundymart-frontend](https://github.com/yourusername/laundymart-frontend)

---

## 🗺️ Roadmap

### Phase 1 (Current)
- [x] User authentication and authorization
- [x] Basic order management
- [x] Role-based access control
- [x] JWT security implementation

### Phase 2 (In Progress)
- [ ] Order status tracking with detailed states
- [ ] Employee & Rider dashboards
- [ ] Advanced admin panel features
- [ ] Order assignment automation

### Phase 3 (Planned)
- [ ] Email/SMS notifications
- [ ] Payment gateway integration
- [ ] File upload for order photos
- [ ] Real-time order tracking
- [ ] Analytics and reporting
- [ ] Multi-language support

### Phase 4 (Future)
- [ ] Mobile app backend support
- [ ] Push notifications
- [ ] Loyalty program integration
- [ ] AI-based pricing recommendations

---

## 🤝 Contributing

Contributions are welcome and appreciated! Here's how you can help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/AmazingFeature`)
3. **Commit** your changes (`git commit -m 'Add some AmazingFeature'`)
4. **Push** to the branch (`git push origin feature/AmazingFeature`)
5. **Open** a Pull Request

### Contribution Guidelines

- Follow Java coding conventions
- Write meaningful commit messages
- Add tests for new features
- Update documentation as needed
- Ensure all tests pass before submitting PR

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2024 LaundryMart

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## 🙏 Acknowledgments

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [JWT.io](https://jwt.io/) - JWT token debugger
- [MySQL Documentation](https://dev.mysql.com/doc/)
- [Maven Repository](https://mvnrepository.com/)
- All contributors and supporters of this project

---

<div align="center">

Made with ❤️ by the LaundryMart Team

⭐ Star this repo if you find it helpful!

</div>
