LaundyMart Backend
Spring Boot
Java
MySQL
JWT
LaundyMart Backend is a robust and secure RESTful API built with Spring Boot 3 for a full-stack laundry management system. It supports multiple user roles with role-based access control, JWT authentication, and order management features.
Features

Role-based Authentication (Admin, Customer, Employee, Rider)
JWT-based stateless authentication
User registration and login
Protected endpoints with Spring Security
Order management (create, track, assign to employee/rider)
CORS enabled for React frontend integration
MySQL database with JPA/Hibernate
Clean layered architecture (Controller → Service → Repository → Entity)

Tech Stack

Framework: Spring Boot 3.2+
Language: Java 17
Security: Spring Security + JJWT
Database: MySQL 8.0
Build Tool: Maven
Dependencies: Lombok, Spring Data JPA, Spring Web

Project Structure
textsrc/main/java/com/laundrymart/backend
├── BackendApplication.java          # Main class
├── config/
│   ├── SecurityConfig.java          # JWT & Security configuration
│   ├── JwtFilter.java               # JWT validation filter
│   └── JwtUtil.java                 # Token generation & validation
├── controller/
│   ├── AuthController.java          # /register & /login
│   ├── AdminController.java         # Admin-only endpoints
│   └── CustomerController.java      # Customer endpoints (e.g., place order)
├── entity/
│   ├── User.java
│   └── Order.java
├── repository/
│   ├── UserRepository.java
│   └── OrderRepository.java
├── service/
│   └── (Business logic services)
└── resources/
    └── application.properties       # DB & JWT config
Prerequisites

Java 17 or higher
MySQL 8.0
Maven 3.8+

Setup & Installation

Clone the repositoryBashgit clone https://github.com/yourusername/laundymart-backend.git
cd laundymart-backend
Configure MySQL
Create a database named laundrymart
Update src/main/resources/application.properties with your MySQL credentials:propertiesspring.datasource.username=root
spring.datasource.password=your_password

Update JWT Secret (Important!)
Replace the placeholder with a strong Base64-encoded secret:propertiesjwt.secret=your_very_long_random_base64_secret_here
Run the applicationBashmvn spring-boot:runThe server will start on http://localhost:8080

API Endpoints
Public Endpoints

POST /register → Register new user
POST /login → Login and receive JWT token

Protected Endpoints (Require JWT in Authorization header)

GET /admin/users → Get all users (Admin only)
POST /customer/orders → Place a new order (Customer only)
More endpoints coming: assign employee/rider, update order status, etc.

Frontend Integration
Designed to work seamlessly with the React frontend:
laundymart-frontend
Security Notes

Passwords are hashed using BCrypt
JWT tokens with HS512 signature
Stateless session management
CORS configured for http://localhost:5173 (Vite)

Future Enhancements

Order status tracking
Employee & Rider dashboards
Admin panel for user/order management
Email notifications
File upload for order photos

Contributing
Contributions are welcome! Feel free to open issues or submit pull requests.
License
MIT License
