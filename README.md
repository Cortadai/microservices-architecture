# 🏗️ Microservices Architecture

4 projects demonstrating Spring Cloud microservices architecture patterns, 
distributed systems design, and centralized configuration management.

## 📚 Projects List

### Infrastructure & Configuration

- **[config-server-repo](https://github.com/Cortadai/config-server-repo)**
  Centralized Configuration Server for Microservices
  - Technology: Spring Boot, Spring Cloud Config
  - Purpose: Centralized management of microservice configurations
  - Scope: 3 main services (Employee, Department, Organization)
  - Data: `.properties` files for each service
  - Services Configured:
    - Employee Service (port 8081) → employee_db
    - Department Service (default port) → department_db
    - Organization Service (port 8083) → organization_db
  - Pattern: Spring Cloud Config Server
  - Benefit: Change configuration without redeploying services
  - Use case: Environment-specific configs (dev, test, prod)

- **[servicio-items-config](https://github.com/Cortadai/servicio-items-config)** (Private)
  Configuration Repository for Items Service
  - Technology: Configuration files (`.properties`)
  - Purpose: Properties storage for Items/Products microservice
  - Status: Private repository
  - Type: Config server repository (Git-based)
  - Usage: Referenced by config-server-repo

### Complete Microservices Systems

- **[springboot-microservices](https://github.com/Cortadai/springboot-microservices)**
  Complete Microservices Ecosystem (Full Stack)
  - Technology: Spring Boot, Spring Cloud, MySQL, Docker
  - Scope: 3 microservices + infrastructure services
  
  **Core Microservices:**
  1. Employee Service (port 8081)
     - Database: MySQL (employee_db)
     - Operations: Employee CRUD
  
  2. Department Service (default port)
     - Database: MySQL (department_db)
     - Operations: Department management
  
  3. Organization Service (port 8083)
     - Database: MySQL (organization_db)
     - Operations: Organization management
  
  **Infrastructure Services:**
  - **Service Registry (Eureka)** - Port 8761
    - Auto-discovery of microservices
    - Load balancing
    - Health checking
  
  - **Config Server** - Port 8888
    - Centralized configuration
    - Dynamic property updates
    - Environment separation
  
  - **API Gateway (Zuul/Spring Cloud Gateway)**
    - Single entry point for clients
    - Routing to appropriate services
    - Load balancing
    - Request filtering
  
  **Advanced Features:**
  - **Communication Patterns:**
    - RestTemplate (Synchronous)
    - WebClient (Non-blocking)
    - OpenFeign (Declarative HTTP client)
  
  - **Resilience Patterns:**
    - Circuit Breaker (Hystrix/Resilience4J)
    - Retry mechanism
    - Rate Limiter
    - Timeout management
  
  - **Observability:**
    - Distributed tracing with Zipkin
    - Centralized logging
    - Metrics with Micrometer
  
  - **Messaging:**
    - RabbitMQ integration
    - Spring Cloud Bus for config refresh
    - Event-driven communication
  
  - **Documentation:**
    - SpringDoc OpenAPI (Swagger)
    - Auto-generated API documentation
  
  - **Deployment:**
    - Docker Compose for local development
    - Container orchestration
    - Network configuration
  
  Use case: Reference implementation of production-grade microservices

- **[springcloud](https://github.com/Cortadai/springcloud)**
  Spring Cloud Microservices System (Legacy - Greenwich)
  - Technology: Spring Cloud Greenwich, Spring Boot, Docker
  - Status: Legacy but fully functional
  - Version: Spring Cloud Greenwich
  
  **Architecture Components:**
  
  **Infrastructure Services:**
  - **Eureka Server** - Service Registry
    - Central service discovery
    - Heartbeat monitoring
  
  - **Config Server** - Centralized Configuration
    - Git-based configuration
    - Dynamic updates
  
  - **Zuul Gateway** - API Gateway
    - Request routing
    - Load balancing
  
  **Business Microservices:**
  - **Products Service**
    - Product catalog management
  
  - **Items Service**
    - Item/Stock management
  
  - **Users Service**
    - User management
  
  - **OAuth Service**
    - Authentication & authorization
  
  **Advanced Features:**
  - **Resilience:**
    - Hystrix circuit breakers
    - Fallback mechanisms
  
  - **Communication:**
    - OpenFeign for service-to-service calls
    - Fault tolerance
  
  - **Distributed Tracing:**
    - Zipkin integration
    - Request flow visualization
  
  - **Messaging:**
    - RabbitMQ for async communication
  
  - **Security:**
    - OAuth2 integration
    - JWT tokens
    - Role-based access control
  
  - **Databases:**
    - MySQL for business data
    - PostgreSQL for users/auth
  
  - **Containerization:**
    - Docker containers for each service
    - Docker Compose orchestration
  
  Note: Uses older Spring Cloud version (Greenwich).
  For new projects, consider using current versions.

---

## 🏗️ Microservices Architecture Patterns

### Service Registry Pattern (Eureka)

```
┌─────────────────────────────────────────┐
│         Eureka Service Registry         │
│  (Service Discovery & Registration)     │
├─────────────────────────────────────────┤
│ Registered Services:                    │
│ ├─ Employee Service (8081)              │
│ ├─ Department Service (8082)            │
│ ├─ Organization Service (8083)          │
│ └─ API Gateway (8080)                   │
│                                         │
│ Provides:                               │
│ ├─ Service location discovery           │
│ ├─ Load balancing                       │
│ └─ Health checking                      │
└─────────────────────────────────────────┘
```

### Config Server Pattern

```
┌──────────────────────────────────────┐
│    Config Server                     │
│    (Centralized Configuration)       │
├──────────────────────────────────────┤
│ Git Repository:                      │
│ ├─ application.properties            │
│ ├─ application-dev.properties        │
│ ├─ application-prod.properties       │
│ └─ service-specific configs          │
│                                      │
│ Provides configs to:                 │
│ ├─ Employee Service                  │
│ ├─ Department Service                │
│ ├─ Organization Service              │
│ └─ ... other services                │
│                                      │
│ Benefits:                            │
│ ├─ No redeploy needed for config     │
│ ├─ Environment separation            │
│ └─ Version control for configs       │
└──────────────────────────────────────┘
```

### API Gateway Pattern

```
┌──────────────────┐
│     Clients      │
│  (Frontend/Apps) │
└────────┬─────────┘
         │
         ▼
┌──────────────────────────────┐
│   API Gateway / Zuul         │
│  (Single Entry Point)        │
│                              │
│  - Route requests            │
│  - Load balance              │
│  - Rate limit                │
│  - Filter/Transform          │
│  - Monitor requests          │
└──────┬───────┬────────┬──────┘
       │       │        │
       ▼       ▼        ▼
┌─────────┐ ┌─────────┐ ┌──────────┐
│Employee │ │Dept.    │ │Org.      │
│Service  │ │Service  │ │Service   │
└─────────┘ └─────────┘ └──────────┘
```

### Service-to-Service Communication

```
Service A (OpenFeign Client)
    │
    ├─ Query Eureka: "Find Service B"
    │
    ├─ Get Service B address
    │
    ├─ Make REST call to Service B
    │
    ├─ Circuit Breaker wraps call
    │   ├─ If success → return response
    │   └─ If failure → fallback mechanism
    │
    └─ Retry with backoff if needed
```

---

## 📊 Project Comparison

| Feature | springboot-microservices | springcloud |
|---------|--------------------------|-------------|
| **Version** | Latest | Greenwich (Legacy) |
| **Services** | 3 core + infrastructure | 4 core + infrastructure |
| **Service Registry** | Eureka | Eureka |
| **Config Server** | Spring Cloud Config | Spring Cloud Config |
| **API Gateway** | Spring Cloud Gateway | Zuul |
| **Circuit Breaker** | Resilience4J | Hystrix |
| **Tracing** | Zipkin | Zipkin |
| **Messaging** | RabbitMQ + Spring Cloud Bus | RabbitMQ |
| **Databases** | MySQL (all) | MySQL + PostgreSQL |
| **Recommended** | ✅ Yes (modern) | ⚠️ Legacy (reference) |

---

## 🛠️ Technologies Stack

### Framework & Runtime
- **Spring Boot 2.7+** / **Spring Boot 3.x** - Application framework
- **Java 11+** / **Java 17+** - Programming language
- **Tomcat** - Embedded web server

### Spring Cloud Components
- **Eureka** - Service registry and discovery
- **Config Server** - Centralized configuration management
- **Spring Cloud Gateway / Zuul** - API Gateway
- **OpenFeign** - Declarative HTTP client
- **Hystrix / Resilience4J** - Circuit breakers
- **Spring Cloud Bus** - Configuration refresh
- **Ribbon** - Client-side load balancing (Eureka)

### Data Access
- **Spring Data JPA** - Data repository abstraction
- **Hibernate** - ORM (Object-Relational Mapping)
- **MySQL 8** - Primary database
- **PostgreSQL** - Alternative/secondary database

### Observability
- **Zipkin** - Distributed tracing
- **Spring Cloud Sleuth** - Trace correlation
- **Micrometer** - Metrics collection
- **ELK Stack** - Logging (optional)

### Messaging
- **RabbitMQ** - Message broker
- **Spring Cloud Bus** - Config refresh notifications
- **Spring AMQP** - RabbitMQ integration

### Development & Deployment
- **Maven** - Build tool
- **Docker** - Containerization
- **Docker Compose** - Container orchestration
- **Git** - Version control

---

## 🚀 Quick Start

### Prerequisites
```bash
Java 11+
Maven 3.6+
Docker & Docker Compose
MySQL 8 (or Docker)
RabbitMQ (or Docker)
```

### Start All Services (Recommended: Docker Compose)

```bash
git clone https://github.com/Cortadai/springboot-microservices.git
cd springboot-microservices

# Start all services
docker-compose up -d

# Services available at:
# - Eureka Dashboard: http://localhost:8761
# - API Gateway: http://localhost:8080
# - Employee Service: http://localhost:8081
# - Config Server: http://localhost:8888
# - RabbitMQ: http://localhost:15672
```

### Start Individual Service

```bash
# Build all
mvn clean install

# Run Employee Service
mvn -pl employee-service spring-boot:run

# Run Department Service
mvn -pl department-service spring-boot:run

# Run Organization Service
mvn -pl organization-service spring-boot:run

# Run API Gateway
mvn -pl api-gateway spring-boot:run
```

### Test the System

```bash
# Get all employees through gateway
curl http://localhost:8080/api/employees

# Get specific employee
curl http://localhost:8080/api/employees/1

# Create new employee
curl -X POST http://localhost:8080/api/employees \
  -H "Content-Type: application/json" \
  -d '{"name":"John","department":"HR"}'

# Check service registry (Eureka)
# Open browser: http://localhost:8761
```

---

## 🎯 Learning Path

### Week 1: Foundation

#### Day 1-2: Understanding Microservices
- What are microservices?
- Monolithic vs Microservices
- Benefits and challenges
- When to use microservices

#### Day 3-4: Service Registry (Eureka)
- Client-side vs Server-side discovery
- Eureka architecture
- Service registration and discovery
- Health checks and heartbeats

#### Day 5: Configuration Management
- Centralized vs distributed config
- Spring Cloud Config Server
- Property sources hierarchy
- Dynamic refresh

### Week 2: Communication & Resilience

#### Day 6-7: Service-to-Service Communication
- Synchronous (REST, HTTP)
- Asynchronous (messaging)
- OpenFeign for declarative clients
- Load balancing strategies

#### Day 8-9: Resilience Patterns
- Circuit Breaker pattern
- Retry mechanisms
- Timeout management
- Fallback strategies
- Bulkhead pattern

#### Day 10: API Gateway
- Gateway responsibilities
- Routing strategies
- Request filtering
- Security at gateway level

### Week 3: Advanced Topics

#### Day 11-12: Distributed Tracing
- Request correlation
- Zipkin integration
- Trace visualization
- Performance analysis

#### Day 13-14: Async Communication
- Event-driven architecture
- RabbitMQ integration
- Spring Cloud Bus
- Configuration refresh

#### Day 15: Monitoring & Operations
- Health checks
- Metrics collection
- Alerting strategies
- Production deployment

---

## 🔗 Service Communication Patterns

### Synchronous (Request-Response)

```java
// Using OpenFeign
@FeignClient(name = "department-service")
public interface DepartmentClient {
    @GetMapping("/departments/{id}")
    Department getDepartment(@PathVariable Long id);
}

// Usage
@Service
public class EmployeeService {
    @Autowired
    private DepartmentClient departmentClient;
    
    public Employee getEmployeeWithDept(Long empId) {
        Employee emp = employeeRepo.findById(empId);
        Department dept = departmentClient.getDepartment(emp.getDeptId());
        emp.setDepartment(dept);
        return emp;
    }
}
```

### Asynchronous (Event-Driven)

```java
// Producer - Employee Service
@Service
public class EmployeeService {
    @Autowired
    private RabbitTemplate rabbitTemplate;
    
    public void createEmployee(Employee emp) {
        employeeRepo.save(emp);
        
        // Publish event
        rabbitTemplate.convertAndSend("employee.exchange", 
            "employee.created", 
            new EmployeeCreatedEvent(emp));
    }
}

// Consumer - Notification Service
@Service
public class NotificationService {
    @RabbitListener(queues = "employee.queue")
    public void handleEmployeeCreated(EmployeeCreatedEvent event) {
        // Send email notification
        sendNotification(event.getEmployee());
    }
}
```

---

## 🏷️ Topics Applied

All projects tagged with:
- `#microservices` - Microservices architecture
- `#spring-cloud` - Spring Cloud framework
- `#learning` - Learning material
- `#tutorial` - Tutorial style
- `#java` - Java language
- `#spring-boot` - Spring Boot framework
- `#containers` - Docker/containerization

Hub project also tagged with:
- `#collection` - Recopilatory hub
- `#architecture` - Architecture patterns
- `#distributed-systems` - Distributed systems

---

## 📊 Project Stats

| Metric | Value |
|--------|-------|
| **Total Projects** | 4 |
| **Microservices** | 3-4 |
| **Infrastructure Services** | 3-4 |
| **Technologies** | Spring Cloud, Eureka, Config Server, Gateway |
| **Learning Level** | Advanced |
| **Estimated Learning Time** | 3 weeks |

---

## 🎓 Learning Outcomes

After completing this collection, you'll understand:

- ✅ Microservices architecture principles
- ✅ Service registry and discovery (Eureka)
- ✅ Centralized configuration management
- ✅ API Gateway patterns and routing
- ✅ Service-to-service communication (sync & async)
- ✅ Resilience patterns (Circuit Breaker, Retry, etc.)
- ✅ Distributed tracing with Zipkin
- ✅ Load balancing strategies
- ✅ Event-driven communication with messaging
- ✅ Configuration refresh without redeployment
- ✅ Docker containerization and orchestration
- ✅ Deployment strategies for microservices
- ✅ Monitoring and observability
- ✅ Security in distributed systems

---

## ⚙️ Configuration Management

### Config Server Setup

```properties
# application.properties (Config Server)
server.port=8888
spring.cloud.config.server.git.uri=https://github.com/user/config-repo
spring.cloud.config.server.git.clone-on-start=true

# Served configs:
# - /application.properties
# - /application-dev.properties
# - /application-prod.properties
# - /employee-service.properties
# - /department-service.properties
```

### Client Configuration

```properties
# application.properties (Config Client)
spring.application.name=employee-service
spring.cloud.config.uri=http://localhost:8888
spring.profiles.active=dev

# Refresh config without restart:
# POST http://localhost:8081/actuator/refresh
```

---

## 🔐 Security Considerations

### Service-to-Service Security
- Service credentials in Config Server
- Network isolation within Docker network
- TLS/SSL encryption for inter-service calls
- OAuth2 for authentication

### API Gateway Security
- Authentication at gateway level
- Rate limiting and throttling
- Request validation and sanitization
- CORS configuration

### Data Security
- Encrypt sensitive data in transit
- Database encryption at rest
- Credential management (Spring Vault, AWS Secrets Manager)

---

## 🧪 Testing Microservices

### Unit Testing
```java
@SpringBootTest
class EmployeeServiceTest {
    @Mock
    private EmployeeRepository employeeRepository;
    
    @InjectMocks
    private EmployeeService employeeService;
    
    @Test
    void testGetEmployee() {
        Long empId = 1L;
        Employee employee = new Employee(...);
        when(employeeRepository.findById(empId)).thenReturn(Optional.of(employee));
        
        Employee result = employeeService.getEmployee(empId);
        assertEquals(employee, result);
    }
}
```

### Integration Testing
```java
@SpringBootTest(webEnvironment = RANDOM_PORT)
class EmployeeServiceIntegrationTest {
    @Autowired
    private TestRestTemplate restTemplate;
    
    @Test
    void testGetEmployeeEndpoint() {
        ResponseEntity<Employee> response = 
            restTemplate.getForEntity("/api/employees/1", Employee.class);
        
        assertEquals(HttpStatus.OK, response.getStatusCode());
        assertNotNull(response.getBody());
    }
}
```

### Contract Testing
- Pact or Spring Cloud Contract for service contracts
- Ensure service compatibility

---

## 🔗 Related Collections

- [Event-Driven & Messaging](link) - Kafka, RabbitMQ patterns
- [Spring Boot Basics](https://github.com/Cortadai/spring-boot-basics) - Foundation concepts
- [Code Generation Tools](link) - WSDL, OpenAPI generation

---

## 💡 Best Practices

### 1. Service Boundaries
- Each service has single responsibility
- Clear API contracts
- Independent data storage
- Loose coupling, high cohesion

### 2. Configuration Management
- Externalize all configuration
- Environment-specific properties
- Secure credential handling
- Version control for configs

### 3. Communication
- Prefer async over sync where possible
- Implement timeouts and retries
- Circuit breakers for resilience
- Proper error handling

### 4. Monitoring
- Distributed tracing for requests
- Metrics collection
- Centralized logging
- Alert thresholds

### 5. Deployment
- Containerize all services
- Use container orchestration
- Blue-green or canary deployments
- Infrastructure as code

---

## 📚 External Resources

### Spring Cloud Official
- [Spring Cloud Documentation](https://spring.io/projects/spring-cloud)
- [Spring Cloud Netflix](https://spring.io/projects/spring-cloud-netflix)
- [Spring Cloud Config](https://spring.io/projects/spring-cloud-config)

### Microservices Patterns
- [microservices.io](https://microservices.io/)
- [Pattern: Service Registry](https://microservices.io/patterns/service-registry.html)
- [Pattern: API Gateway](https://microservices.io/patterns/apigateway.html)

### Tools & Platforms
- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [Zipkin](https://zipkin.io/)
- [Eureka](https://github.com/Netflix/eureka)

---

## 📬 Notes

These projects demonstrate complete microservices architectures using Spring Cloud.

**springboot-microservices** is recommended for modern development (latest Spring Cloud).
**springcloud** provides reference for legacy Greenwich version but remains fully functional.

Perfect for:
- ✅ Understanding microservices patterns
- ✅ Learning distributed systems design
- ✅ Enterprise architecture implementation
- ✅ Production-ready service design
- ✅ Team learning and reference

*Last updated: November 2025*
