# 🏗️ Arquitectura de Microservicios

4 proyectos que demuestran patrones de arquitectura de microservicios con Spring Cloud, diseño de sistemas distribuidos y gestión centralizada de configuración.

## 📚 Lista de Proyectos

### Infraestructura y Configuración

- **[config-server-repo](https://github.com/Cortadai/config-server-repo)**  
  Servidor de Configuración Centralizada para Microservicios
  - Tecnología: Spring Boot, Spring Cloud Config
  - Propósito: Gestión centralizada de configuraciones de microservicios
  - Alcance: 3 servicios principales (Employee, Department, Organization)
  - Datos: Archivos `.properties` para cada servicio
  - Servicios Configurados:
    - Employee Service (puerto 8081) → employee_db
    - Department Service (puerto por defecto) → department_db
    - Organization Service (puerto 8083) → organization_db
  - Patrón: Spring Cloud Config Server
  - Beneficio: Cambiar configuración sin redesplegar servicios
  - Caso de uso: Configuraciones específicas por entorno (dev, test, prod)

- **[servicio-items-config](https://github.com/Cortadai/servicio-items-config)** (Privado)  
  Repositorio de Configuración para el Servicio de Items
  - Tecnología: Archivos de configuración (`.properties`)
  - Propósito: Almacenamiento de propiedades para microservicio Items/Productos
  - Estado: Repositorio privado
  - Tipo: Repositorio de servidor de configuración (basado en Git)
  - Uso: Referenciado por config-server-repo

### Sistemas Completos de Microservicios

- **[springboot-microservices](https://github.com/Cortadai/springboot-microservices)**  
  Ecosistema Completo de Microservicios (Full Stack)
  - Tecnología: Spring Boot, Spring Cloud, MySQL, Docker
  - Alcance: 3 microservicios + servicios de infraestructura
  
  **Microservicios Core:**
  1. Employee Service (puerto 8081)
     - Base de datos: MySQL (employee_db)
     - Operaciones: CRUD de empleados
  
  2. Department Service (puerto por defecto)
     - Base de datos: MySQL (department_db)
     - Operaciones: Gestión de departamentos
  
  3. Organization Service (puerto 8083)
     - Base de datos: MySQL (organization_db)
     - Operaciones: Gestión de organizaciones
  
  **Servicios de Infraestructura:**
  - **Service Registry (Eureka)** - Puerto 8761
    - Autodescubrimiento de microservicios
    - Balanceo de carga
    - Comprobación de salud
  
  - **Config Server** - Puerto 8888
    - Configuración centralizada
    - Actualizaciones dinámicas de propiedades
    - Separación por entornos
  
  - **API Gateway (Zuul/Spring Cloud Gateway)**
    - Punto de entrada único para clientes
    - Enrutamiento a servicios apropiados
    - Balanceo de carga
    - Filtrado de peticiones
  
  **Características Avanzadas:**
  - **Patrones de Comunicación:**
    - RestTemplate (Síncrono)
    - WebClient (No bloqueante)
    - OpenFeign (Cliente HTTP declarativo)
  
  - **Patrones de Resiliencia:**
    - Circuit Breaker (Hystrix/Resilience4J)
    - Mecanismo de reintentos
    - Rate Limiter
    - Gestión de timeouts
  
  - **Observabilidad:**
    - Trazabilidad distribuida con Zipkin
    - Logging centralizado
    - Métricas con Micrometer
  
  - **Mensajería:**
    - Integración con RabbitMQ
    - Spring Cloud Bus para refresco de configuración
    - Comunicación orientada a eventos
  
  - **Documentación:**
    - SpringDoc OpenAPI (Swagger)
    - Documentación de API autogenerada
  
  - **Despliegue:**
    - Docker Compose para desarrollo local
    - Orquestación de contenedores
    - Configuración de red
  
  Caso de uso: Implementación de referencia de microservicios de grado de producción

- **[springcloud](https://github.com/Cortadai/springcloud)**  
  Sistema de Microservicios Spring Cloud (Legacy - Greenwich)
  - Tecnología: Spring Cloud Greenwich, Spring Boot, Docker
  - Estado: Legacy pero totalmente funcional
  - Versión: Spring Cloud Greenwich
  
  **Componentes de Arquitectura:**
  
  **Servicios de Infraestructura:**
  - **Eureka Server** - Service Registry
    - Descubrimiento centralizado de servicios
    - Monitorización de latidos (heartbeat)
  
  - **Config Server** - Configuración Centralizada
    - Configuración basada en Git
    - Actualizaciones dinámicas
  
  - **Zuul Gateway** - API Gateway
    - Enrutamiento de peticiones
    - Balanceo de carga
  
  **Microservicios de Negocio:**
  - **Products Service**
    - Gestión de catálogo de productos
  
  - **Items Service**
    - Gestión de items/stock
  
  - **Users Service**
    - Gestión de usuarios
  
  - **OAuth Service**
    - Autenticación y autorización
  
  **Características Avanzadas:**
  - **Resiliencia:**
    - Circuit breakers con Hystrix
    - Mecanismos de fallback
  
  - **Comunicación:**
    - OpenFeign para llamadas entre servicios
    - Tolerancia a fallos
  
  - **Trazabilidad Distribuida:**
    - Integración con Zipkin
    - Visualización de flujo de peticiones
  
  - **Mensajería:**
    - RabbitMQ para comunicación asíncrona
  
  - **Seguridad:**
    - Integración con OAuth2
    - Tokens JWT
    - Control de acceso basado en roles
  
  - **Bases de Datos:**
    - MySQL para datos de negocio
    - PostgreSQL para usuarios/autenticación
  
  - **Contenerización:**
    - Contenedores Docker para cada servicio
    - Orquestación con Docker Compose
  
  Nota: Usa versión antigua de Spring Cloud (Greenwich).  
  Para proyectos nuevos, considerar usar versiones actuales.

---

## 🏗️ Patrones de Arquitectura de Microservicios

### Patrón Service Registry (Eureka)
```
┌─────────────────────────────────────────┐
│     Eureka Service Registry             │
│  (Descubrimiento y Registro)            │
├─────────────────────────────────────────┤
│ Servicios Registrados:                  │
│ ├─ Employee Service (8081)              │
│ ├─ Department Service (8082)            │
│ ├─ Organization Service (8083)          │
│ └─ API Gateway (8080)                   │
│                                         │
│ Proporciona:                            │
│ ├─ Descubrimiento de localización       │
│ ├─ Balanceo de carga                    │
│ └─ Comprobación de salud                │
└─────────────────────────────────────────┘
```

### Patrón Config Server
```
┌──────────────────────────────────────┐
│    Config Server                     │
│    (Configuración Centralizada)      │
├──────────────────────────────────────┤
│ Repositorio Git:                     │
│ ├─ application.properties            │
│ ├─ application-dev.properties        │
│ ├─ application-prod.properties       │
│ └─ configs específicos por servicio  │
│                                      │
│ Proporciona configs a:               │
│ ├─ Employee Service                  │
│ ├─ Department Service                │
│ ├─ Organization Service              │
│ └─ ... otros servicios               │
│                                      │
│ Beneficios:                          │
│ ├─ Sin redespliegue para cambios     │
│ ├─ Separación por entornos           │
│ └─ Control de versiones para configs │
└──────────────────────────────────────┘
```

### Patrón API Gateway
```
┌──────────────────┐
│     Clientes     │
│  (Frontend/Apps) │
└────────┬─────────┘
         │
         ▼
┌──────────────────────────────┐
│   API Gateway / Zuul         │
│  (Punto de Entrada Único)    │
│                              │
│  - Enrutamiento              │
│  - Balanceo de carga         │
│  - Rate limiting             │
│  - Filtrado/Transformación   │
│  - Monitorización            │
└──────┬───────┬────────┬──────┘
       │       │        │
       ▼       ▼        ▼
┌─────────┐ ┌─────────┐ ┌──────────┐
│Employee │ │Dept.    │ │Org.      │
│Service  │ │Service  │ │Service   │
└─────────┘ └─────────┘ └──────────┘
```

### Comunicación Entre Servicios
```
Servicio A (Cliente OpenFeign)
    │
    ├─ Consulta Eureka: "Buscar Servicio B"
    │
    ├─ Obtener dirección del Servicio B
    │
    ├─ Realizar llamada REST al Servicio B
    │
    ├─ Circuit Breaker envuelve la llamada
    │   ├─ Si éxito → devolver respuesta
    │   └─ Si fallo → mecanismo de fallback
    │
    └─ Reintentar con backoff si es necesario
```

---

## 📊 Comparativa de Proyectos

| Característica | springboot-microservices | springcloud |
|----------------|--------------------------|-------------|
| **Versión** | Última | Greenwich (Legacy) |
| **Servicios** | 3 core + infraestructura | 4 core + infraestructura |
| **Service Registry** | Eureka | Eureka |
| **Config Server** | Spring Cloud Config | Spring Cloud Config |
| **API Gateway** | Spring Cloud Gateway | Zuul |
| **Circuit Breaker** | Resilience4J | Hystrix |
| **Trazabilidad** | Zipkin | Zipkin |
| **Mensajería** | RabbitMQ + Spring Cloud Bus | RabbitMQ |
| **Bases de Datos** | MySQL (todas) | MySQL + PostgreSQL |
| **Recomendado** | ✅ Sí (moderno) | ⚠️ Legacy (referencia) |

---

## 🛠️ Stack Tecnológico

### Framework y Runtime
- **Spring Boot 2.7+** / **Spring Boot 3.x** - Framework de aplicación
- **Java 11+** / **Java 17+** - Lenguaje de programación
- **Tomcat** - Servidor web embebido

### Componentes Spring Cloud
- **Eureka** - Registro y descubrimiento de servicios
- **Config Server** - Gestión centralizada de configuración
- **Spring Cloud Gateway / Zuul** - API Gateway
- **OpenFeign** - Cliente HTTP declarativo
- **Hystrix / Resilience4J** - Circuit breakers
- **Spring Cloud Bus** - Refresco de configuración
- **Ribbon** - Balanceo de carga del lado del cliente (Eureka)

### Acceso a Datos
- **Spring Data JPA** - Abstracción de repositorios de datos
- **Hibernate** - ORM (Object-Relational Mapping)
- **MySQL 8** - Base de datos principal
- **PostgreSQL** - Base de datos alternativa/secundaria

### Observabilidad
- **Zipkin** - Trazabilidad distribuida
- **Spring Cloud Sleuth** - Correlación de trazas
- **Micrometer** - Recolección de métricas
- **ELK Stack** - Logging (opcional)

### Mensajería
- **RabbitMQ** - Message broker
- **Spring Cloud Bus** - Notificaciones de refresco de configuración
- **Spring AMQP** - Integración con RabbitMQ

### Desarrollo y Despliegue
- **Maven** - Herramienta de construcción
- **Docker** - Contenerización
- **Docker Compose** - Orquestación de contenedores
- **Git** - Control de versiones

---

## 🚀 Inicio Rápido

### Prerrequisitos
```bash
Java 11+
Maven 3.6+
Docker & Docker Compose
MySQL 8 (o Docker)
RabbitMQ (o Docker)
```

### Iniciar Todos los Servicios (Recomendado: Docker Compose)
```bash
git clone https://github.com/Cortadai/springboot-microservices.git
cd springboot-microservices

# Iniciar todos los servicios
docker-compose up -d

# Servicios disponibles en:
# - Dashboard Eureka: http://localhost:8761
# - API Gateway: http://localhost:8080
# - Employee Service: http://localhost:8081
# - Config Server: http://localhost:8888
# - RabbitMQ: http://localhost:15672
```

### Iniciar Servicio Individual
```bash
# Construir todo
mvn clean install

# Ejecutar Employee Service
mvn -pl employee-service spring-boot:run

# Ejecutar Department Service
mvn -pl department-service spring-boot:run

# Ejecutar Organization Service
mvn -pl organization-service spring-boot:run

# Ejecutar API Gateway
mvn -pl api-gateway spring-boot:run
```

### Probar el Sistema
```bash
# Obtener todos los empleados a través del gateway
curl http://localhost:8080/api/employees

# Obtener empleado específico
curl http://localhost:8080/api/employees/1

# Crear nuevo empleado
curl -X POST http://localhost:8080/api/employees \
  -H "Content-Type: application/json" \
  -d '{"name":"John","department":"HR"}'

# Comprobar registro de servicios (Eureka)
# Abrir navegador: http://localhost:8761
```

---

## 🎯 Ruta de Aprendizaje

### Semana 1: Fundamentos

#### Día 1-2: Entendiendo Microservicios
- ¿Qué son los microservicios?
- Monolítico vs Microservicios
- Beneficios y desafíos
- Cuándo usar microservicios

#### Día 3-4: Service Registry (Eureka)
- Descubrimiento del lado del cliente vs servidor
- Arquitectura de Eureka
- Registro y descubrimiento de servicios
- Comprobaciones de salud y latidos (heartbeats)

#### Día 5: Gestión de Configuración
- Configuración centralizada vs distribuida
- Spring Cloud Config Server
- Jerarquía de fuentes de propiedades
- Refresco dinámico

### Semana 2: Comunicación y Resiliencia

#### Día 6-7: Comunicación Entre Servicios
- Síncrona (REST, HTTP)
- Asíncrona (mensajería)
- OpenFeign para clientes declarativos
- Estrategias de balanceo de carga

#### Día 8-9: Patrones de Resiliencia
- Patrón Circuit Breaker
- Mecanismos de reintento
- Gestión de timeouts
- Estrategias de fallback
- Patrón Bulkhead

#### Día 10: API Gateway
- Responsabilidades del gateway
- Estrategias de enrutamiento
- Filtrado de peticiones
- Seguridad a nivel de gateway

### Semana 3: Temas Avanzados

#### Día 11-12: Trazabilidad Distribuida
- Correlación de peticiones
- Integración con Zipkin
- Visualización de trazas
- Análisis de rendimiento

#### Día 13-14: Comunicación Asíncrona
- Arquitectura orientada a eventos
- Integración con RabbitMQ
- Spring Cloud Bus
- Refresco de configuración

#### Día 15: Monitorización y Operaciones
- Comprobaciones de salud
- Recolección de métricas
- Estrategias de alertas
- Despliegue en producción

---

## 🔗 Patrones de Comunicación Entre Servicios

### Síncrona (Petición-Respuesta)
```java
// Usando OpenFeign
@FeignClient(name = "department-service")
public interface DepartmentClient {
    @GetMapping("/departments/{id}")
    Department getDepartment(@PathVariable Long id);
}

// Uso
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

### Asíncrona (Orientada a Eventos)
```java
// Productor - Employee Service
@Service
public class EmployeeService {
    @Autowired
    private RabbitTemplate rabbitTemplate;
    
    public void createEmployee(Employee emp) {
        employeeRepo.save(emp);
        
        // Publicar evento
        rabbitTemplate.convertAndSend("employee.exchange", 
            "employee.created", 
            new EmployeeCreatedEvent(emp));
    }
}

// Consumidor - Notification Service
@Service
public class NotificationService {
    @RabbitListener(queues = "employee.queue")
    public void handleEmployeeCreated(EmployeeCreatedEvent event) {
        // Enviar notificación por email
        sendNotification(event.getEmployee());
    }
}
```

---

## 🏷️ Topics Aplicados

Todos los proyectos etiquetados con:
- `#microservicios` - Arquitectura de microservicios
- `#spring-cloud` - Framework Spring Cloud
- `#aprendizaje` - Material de aprendizaje
- `#tutorial` - Estilo tutorial
- `#java` - Lenguaje Java
- `#spring-boot` - Framework Spring Boot
- `#contenedores` - Docker/contenerización

Proyecto hub también etiquetado con:
- `#coleccion` - Hub recopilatorio
- `#arquitectura` - Patrones de arquitectura
- `#sistemas-distribuidos` - Sistemas distribuidos

---

## 📊 Estadísticas del Proyecto

| Métrica | Valor |
|---------|-------|
| **Total Proyectos** | 4 |
| **Microservicios** | 3-4 |
| **Servicios de Infraestructura** | 3-4 |
| **Tecnologías** | Spring Cloud, Eureka, Config Server, Gateway |
| **Nivel de Aprendizaje** | Avanzado |
| **Tiempo Estimado de Aprendizaje** | 3 semanas |

---

## 🎓 Resultados de Aprendizaje

Después de completar esta colección, comprenderás:
- ✅ Principios de arquitectura de microservicios
- ✅ Registro y descubrimiento de servicios (Eureka)
- ✅ Gestión centralizada de configuración
- ✅ Patrones de API Gateway y enrutamiento
- ✅ Comunicación entre servicios (síncrona y asíncrona)
- ✅ Patrones de resiliencia (Circuit Breaker, Retry, etc.)
- ✅ Trazabilidad distribuida con Zipkin
- ✅ Estrategias de balanceo de carga
- ✅ Comunicación orientada a eventos con mensajería
- ✅ Refresco de configuración sin redespliegue
- ✅ Contenerización con Docker y orquestación
- ✅ Estrategias de despliegue para microservicios
- ✅ Monitorización y observabilidad
- ✅ Seguridad en sistemas distribuidos

---

## ⚙️ Gestión de Configuración

### Configuración del Config Server
```properties
# application.properties (Config Server)
server.port=8888
spring.cloud.config.server.git.uri=https://github.com/user/config-repo
spring.cloud.config.server.git.clone-on-start=true

# Configuraciones servidas:
# - /application.properties
# - /application-dev.properties
# - /application-prod.properties
# - /employee-service.properties
# - /department-service.properties
```

### Configuración del Cliente
```properties
# application.properties (Config Client)
spring.application.name=employee-service
spring.cloud.config.uri=http://localhost:8888
spring.profiles.active=dev

# Refrescar config sin reiniciar:
# POST http://localhost:8081/actuator/refresh
```

---

## 🔐 Consideraciones de Seguridad

### Seguridad Entre Servicios
- Credenciales de servicio en Config Server
- Aislamiento de red dentro de red Docker
- Cifrado TLS/SSL para llamadas entre servicios
- OAuth2 para autenticación

### Seguridad del API Gateway
- Autenticación a nivel de gateway
- Rate limiting y throttling
- Validación y sanitización de peticiones
- Configuración CORS

### Seguridad de Datos
- Cifrar datos sensibles en tránsito
- Cifrado de base de datos en reposo
- Gestión de credenciales (Spring Vault, AWS Secrets Manager)

---

## 🧪 Testing de Microservicios

### Testing Unitario
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

### Testing de Integración
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

### Testing de Contratos
- Pact o Spring Cloud Contract para contratos de servicio
- Asegurar compatibilidad entre servicios

---

## 🔗 Colecciones Relacionadas

- [Event-Driven & Messaging](https://github.com/Cortadai/event-driven-messaging-architecture) - Patrones Kafka, RabbitMQ
- [Spring Boot Basics](https://github.com/Cortadai/spring-boot-basics) - Conceptos fundamentales
- [Code Generation Tools](https://github.com/Cortadai/code-generation-tools) - Generación WSDL, OpenAPI

---

## 💡 Mejores Prácticas

### 1. Fronteras de Servicios
- Cada servicio tiene responsabilidad única
- Contratos de API claros
- Almacenamiento de datos independiente
- Bajo acoplamiento, alta cohesión

### 2. Gestión de Configuración
- Externalizar toda la configuración
- Propiedades específicas por entorno
- Manejo seguro de credenciales
- Control de versiones para configuraciones

### 3. Comunicación
- Preferir asíncrona sobre síncrona donde sea posible
- Implementar timeouts y reintentos
- Circuit breakers para resiliencia
- Manejo adecuado de errores

### 4. Monitorización
- Trazabilidad distribuida para peticiones
- Recolección de métricas
- Logging centralizado
- Umbrales de alertas

### 5. Despliegue
- Contenerizar todos los servicios
- Usar orquestación de contenedores
- Despliegues blue-green o canary
- Infraestructura como código

---

## 📚 Recursos Externos

### Spring Cloud Oficial
- [Documentación Spring Cloud](https://spring.io/projects/spring-cloud)
- [Spring Cloud Netflix](https://spring.io/projects/spring-cloud-netflix)
- [Spring Cloud Config](https://spring.io/projects/spring-cloud-config)

### Patrones de Microservicios
- [microservices.io](https://microservices.io/)
- [Patrón: Service Registry](https://microservices.io/patterns/service-registry.html)
- [Patrón: API Gateway](https://microservices.io/patterns/apigateway.html)

### Herramientas y Plataformas
- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [Zipkin](https://zipkin.io/)
- [Eureka](https://github.com/Netflix/eureka)

---

## 📬 Notas

Estos proyectos demuestran arquitecturas completas de microservicios usando Spring Cloud.

**springboot-microservices** es recomendado para desarrollo moderno (última versión de Spring Cloud).

**springcloud** proporciona referencia para la versión legacy Greenwich pero permanece totalmente funcional.

Perfecto para:
- ✅ Entender patrones de microservicios
- ✅ Aprender diseño de sistemas distribuidos
- ✅ Implementación de arquitectura empresarial
- ✅ Diseño de servicios listos para producción
- ✅ Aprendizaje y referencia de equipo

*Última actualización: Noviembre 2025*
