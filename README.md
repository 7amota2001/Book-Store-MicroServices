# Book Store MicroServices

A microservices-based online course management system (similar to a book store but for courses) built with Java, Spring Boot, and Jakarta EE. The system allows users to manage courses, enroll in courses, submit reviews, and manage user accounts.

## 📋 Table of Contents

- [Architecture](#architecture)
- [Technology Stack](#technology-stack)
- [Services](#services)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [API Endpoints](#api-endpoints)
- [Project Structure](#project-structure)

## 🏗 Architecture

This application follows a microservices architecture with two main services:

1. **Course Microservice** - Handles all course-related operations, enrollments, and reviews
2. **User and Course Management Service** - Manages user authentication, authorization, and user-course interactions

Each service has its own database:
- Course Microservice uses **PostgreSQL**
- User Management Service uses **MySQL**

## 🛠 Technology Stack

### Course Microservice
- **Framework**: Spring Boot 2.6.4
- **Language**: Java 17
- **Database**: PostgreSQL
- **ORM**: Spring Data JPA with Hibernate 5.6.1
- **Build Tool**: Maven
- **Authentication**: JWT (Auth0 Java-JWT)
- **Additional Libraries**: 
  - Lombok
  - Gson
  - Jackson

### User and Course Management Service
- **Framework**: Jakarta EE 9.1
- **Language**: Java 11
- **Database**: MySQL 8.0.28
- **ORM**: Hibernate 5.6.4
- **Application Server**: WildFly (via Docker)
- **Build Tool**: Maven
- **Authentication**: JWT (JJWT 0.11.2)
- **Packaging**: WAR

### DevOps
- **Containerization**: Docker
- **Base Image**: WildFly (quay.io/wildfly/wildfly:latest)

## 🔧 Services

### 1. Course Microservice (Port 8090)

Manages all course-related operations:
- Course CRUD operations
- Course search by name and category
- Course sorting by rating
- Student enrollment management
- Course reviews

### 2. User and Course Management Service (Port 8080)

Handles user management and authentication:
- User registration and authentication
- User profile management
- JWT-based authentication
- Integration with Course Microservice

## 📦 Prerequisites

Before running this application, ensure you have the following installed:

- **Java Development Kit (JDK)**:
  - JDK 17 (for Course Microservice)
  - JDK 11 (for User Management Service)
- **Maven**: 3.6 or higher
- **PostgreSQL**: 12 or higher
- **MySQL**: 8.0 or higher
- **Docker**: (Optional, for containerized deployment)
- **WildFly**: Latest version (if not using Docker)

## 💻 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/7amota2001/Book-Store-MicroServices.git
cd Book-Store-MicroServices
```

### 2. Database Setup

#### PostgreSQL (Course Microservice)

```sql
CREATE DATABASE courseDB;
```

#### MySQL (User Management Service)

```sql
CREATE DATABASE userDB;
```

## ⚙️ Configuration

### Course Microservice Configuration

Edit `course-microservice/src/main/resources/application.properties`:

```properties
spring.application.name=course-microservice
server.port=8090

# PostgreSQL Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/courseDB
spring.datasource.username=postgres
spring.datasource.password=YOUR_PASSWORD
spring.datasource.driver-class-name=org.postgresql.Driver

spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=update
```

### User Management Service Configuration

Edit `resources/META-INF/persistence.xml`:

```xml
<!-- MySQL database connection properties -->
<property name="javax.persistence.jdbc.driver" value="com.mysql.cj.jdbc.Driver" />
<property name="javax.persistence.jdbc.url" value="jdbc:mysql://localhost:3306/userDB" />
<property name="javax.persistence.jdbc.user" value="root" />
<property name="javax.persistence.jdbc.password" value="YOUR_PASSWORD" />
```

## 🚀 Running the Application

### Option 1: Run Services Individually

#### Course Microservice

```bash
cd course-microservice
./mvnw spring-boot:run
```

The service will start on `http://localhost:8090`

#### User Management Service

```bash
# Build the project
mvn clean package

# Deploy to WildFly (if using local WildFly installation)
# Copy the WAR file to WildFly deployments directory
cp target/UserAndCourseManagement-1.0-SNAPSHOT.war $WILDFLY_HOME/standalone/deployments/
```

The service will start on `http://localhost:8080`

### Option 2: Using Docker

```bash
# Build the User Management Service
mvn clean package

# Build and run with Docker
docker build -t user-course-management .
docker run -p 8080:8080 user-course-management
```

## 📡 API Endpoints

### Course Microservice (`http://localhost:8090`)

#### Course Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/courses/add` | Add a new course |
| GET | `/courses/all` | Get all courses |
| GET | `/courses/{id}` | Get course by ID |
| PUT | `/courses/update/{id}` | Update course |
| DELETE | `/courses/delete/{id}` | Delete course |
| GET | `/courses/searchByName?name={name}` | Search courses by name |
| GET | `/courses/searchByCategory?category={category}` | Search courses by category |
| GET | `/courses/sortByRating` | Get courses sorted by rating |

#### Enrollment Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/courses/enrollments/add?courseID={courseID}&studentID={studentID}` | Enroll student in course |
| PUT | `/courses/enrollments/update/{id}` | Update enrollment status |
| DELETE | `/courses/enrollments/delete/{id}` | Delete enrollment |

#### Review Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/courses/reviews/add?courseID={courseID}&studentID={studentID}` | Add course review |
| GET | `/courses/reviews/all` | Get all reviews |
| GET | `/courses/reviews?courseID={courseID}` | Get reviews by course ID |

### User Management Service (`http://localhost:8080`)

#### User Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/users/signUp` | Register a new user |
| GET | `/users/getAllUsers` | Get all users |
| POST | `/users/login` | User login (returns JWT token) |
| GET | `/users/profile` | Get user profile |
| PUT | `/users/update` | Update user profile |

## 📁 Project Structure

```
Book-Store-MicroServices/
├── course-microservice/          # Spring Boot microservice for courses
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── com/example/coursemicroservice/
│   │   │   │       ├── CourseMicroserviceApplication.java
│   │   │   │       ├── CourseController.java
│   │   │   │       ├── Course.java
│   │   │   │       ├── CourseRepository.java
│   │   │   │       ├── Enrollment.java
│   │   │   │       ├── EnrollmentRepository.java
│   │   │   │       ├── Review.java
│   │   │   │       ├── ReviewRepository.java
│   │   │   │       └── DatabaseConfig.java
│   │   │   └── resources/
│   │   │       └── application.properties
│   │   └── test/
│   ├── pom.xml
│   ├── mvnw
│   └── mvnw.cmd
├── userandcoursemanagement/      # Jakarta EE service for user management
│   ├── Users.java
│   ├── UserController.java
│   ├── Course.java
│   ├── Review.java
│   ├── HelloApplication.java
│   ├── HelloResource.java
│   ├── SpringBootAPIClient.java
│   ├── courseService.java
│   └── CorsFilter.java
├── resources/
│   └── META-INF/
│       ├── persistence.xml       # JPA configuration
│       ├── beans.xml
│       └── jboss-ds.xml
├── Dockerfile                    # Docker configuration for User Management Service
├── pom.xml                       # Parent POM for User Management Service
├── mvnw
└── mvnw.cmd
```

## 🔐 Security

The application uses JWT (JSON Web Tokens) for authentication:
- Users must authenticate to receive a JWT token
- Protected endpoints require a valid JWT token in the Authorization header
- CORS is configured to allow cross-origin requests

## 🗃️ Database Schema

### Course Database (PostgreSQL)

- **courses**: Stores course information
- **enrollments**: Tracks student enrollments
- **reviews**: Stores course reviews

### User Database (MySQL)

- **usertable**: Stores user information and credentials

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is available for educational and personal use.

## 👨‍💻 Author

**7amota2001**

## 📞 Support

For support, please open an issue in the GitHub repository.

---

**Note**: Remember to update database credentials in configuration files before running the application.
