E-commerce Demo (Spring Boot)
A lightweight cloud-native e-commerce demo built with Spring Boot, PostgreSQL, JPA, Flyway, and a simple static frontend.
It demonstrates user authentication, product browsing, and order creation with quantity-based items.

Features
User registration and login

Token-based authentication (Bearer token)

Product listing

Create orders with flexible payload:

List<OrderItemRequest> (productId, quantity)

View current user’s orders

Database migration with Flyway

Swagger/OpenAPI documentation

Tech Stack
Java 17+

Spring Boot

Spring Web

Spring Data JPA

PostgreSQL

Flyway

Jakarta Validation

Swagger/OpenAPI

Vanilla HTML/CSS/JS frontend

Project Structure
src/main/java/com/huawei/cloudnative/ecommerce
├── config          # exception handling, OpenAPI config
├── controller      # REST APIs
├── dto             # request/response DTOs
├── entity          # JPA entities
├── model           # API response models
├── repository      # Spring Data repositories
└── service         # business logic

src/main/resources
├── application.yml
├── db/migration    # Flyway SQL migrations
└── static          # demo frontend (index.html, app.js, styles.css)
Quick Start
1) Clone and enter project
git clone <your-repo-url>
cd E-commerce-huawei
2) Configure database
Make sure PostgreSQL is running and configure connection values in:

src/main/resources/application.yml

Typical required values:

host

port

database name

username

password

3) Run the application
mvn spring-boot:run
Or package and run:

mvn clean package
java -jar target/*.jar
Application default URL:

http://localhost:8080

API Documentation
Swagger UI:

http://localhost:8080/swagger-ui.html

Authentication Flow
Register a new user via /api/auth/register

Receive a token in response

Use token as Bearer token in Authorization header for protected APIs

Header format:

Authorization: Bearer <token>
Core APIs
1) Register
POST /api/auth/register

Request:

{
  "username": "alice",
  "password": "password123"
}
Response (example):

{
  "token": "xxxx.yyyy.zzzz",
  "username": "alice"
}
2) Login
POST /api/auth/login

Request:

{
  "username": "alice",
  "password": "password123"
}
Response:

{
  "token": "xxxx.yyyy.zzzz",
  "username": "alice"
}
3) List Products
GET /api/products

Response (example):

[
  { "id": 1, "name": "Huawei Watch", "description": "Smart watch for fitness and productivity", "price": 1999.00 },
  { "id": 2, "name": "Wireless Earbuds", "description": "Noise cancellation earbuds", "price": 799.00 }
]
4) Create Order
POST /api/orders
Requires Bearer token.

Request:

{
  "items": [
    { "productId": 1, "quantity": 2 },
    { "productId": 2, "quantity": 1 }
  ]
}
Response (example):

{
  "id": 10,
  "username": "alice",
  "productIds": [1, 2],
  "totalAmount": 4797.00,
  "createdAt": "2026-04-21T10:30:00Z"
}
5) Load My Orders
GET /api/orders
Requires Bearer token.

Response:

[
  {
    "id": 10,
    "username": "alice",
    "productIds": [1, 2],
    "totalAmount": 4797.00,
    "createdAt": "2026-04-21T10:30:00Z"
  }
]
Frontend Demo
Open:

http://localhost:8080

Demo UI supports:

Register

Login

Load products

Create order using productId:quantity format (e.g. 1:2,2:1)

Load current user orders

Validation & Error Handling
Request validation enabled via Jakarta Validation

Typical failures:

invalid credentials

duplicate username

missing/invalid order items

invalid token / unauthorized access

Notes for Live Demo
Recommended demo sequence:

Start app + show Swagger

Register a new user

Login and copy token

Load product list

Create order with multiple quantities

Load my orders to show persisted history

Highlight transaction-safe order creation and DTO-based request design

Future Improvements
Password hashing (e.g., BCrypt) instead of plain text storage

Better order response model including item details (quantity, unitPrice)

More granular exception mapping (400/401/404/409)

Integration tests (Testcontainers + PostgreSQL)

Inventory/stock control and payment workflow
