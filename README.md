# E-Commerce API

Uma API RESTful completa de e-commerce desenvolvida com **Spring Boot**, demonstrando boas práticas de desenvolvimento backend, persistência de dados com JPA/Hibernate e padrões de arquitetura profissionais.

## 🎯 Visão Geral

Este projeto é uma aplicação backend robusta para um sistema de e-commerce, com funcionalidades completas para gerenciamento de:
- **Usuários** (Clientes)
- **Produtos** e **Categorias**
- **Pedidos** (Orders) e **Itens de Pedido**
- **Pagamentos**

## 🏗️ Arquitetura

O projeto segue o padrão de **arquitetura em camadas**:

```
┌─────────────────────────────────────────┐
│         REST Controllers (API)          │
│      (Resources - Endpoints HTTP)       │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│          Services Layer                 │
│    (Lógica de Negócio & Validações)     │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│       Repositories (Spring Data JPA)    │
│      (Acesso aos Dados - ORM)           │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│         Banco de Dados (H2/PostgreSQL)  │
└─────────────────────────────────────────┘
```

## 🛠️ Tecnologias Utilizadas

- **Java 25** - Linguagem de programação
- **Spring Boot 4.0.2** - Framework principal
- **Spring Data JPA** - Abstração de dados e ORM
- **Hibernate** - Mapeamento objeto-relacional
- **H2 Database** - Banco de dados em memória (desenvolvimento)
- **PostgreSQL** - Banco de dados (produção)
- **Maven** - Gerenciador de dependências e build

## 📦 Dependências Principais

```xml
<!-- Spring Boot Web MVC -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webmvc</artifactId>
</dependency>

<!-- Spring Data JPA -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<!-- H2 Console -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-h2console</artifactId>
</dependency>

<!-- PostgreSQL Driver -->
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

## 📊 Modelo de Dados

### Entidades Principais

#### User (tb_user)
Representa um usuário/cliente do sistema.

| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | Long | Identificador único (PK) |
| name | String | Nome do usuário |
| email | String | Email do usuário |
| phone | String | Telefone |
| password | String | Senha (criptografada) |

**Relacionamentos:**
- One-to-Many com Order (1 usuário → muitos pedidos)

#### Product (tb_product)
Representa um produto disponível no catálogo.

| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | Long | Identificador único (PK) |
| name | String | Nome do produto |
| description | String | Descrição detalhada |
| price | Double | Preço unitário |
| imgUrl | String | URL da imagem |

**Relacionamentos:**
- Many-to-Many com Category (via tb_product_category)
- One-to-Many com OrderItem

#### Category (tb_category)
Representa uma categoria de produtos.

**Relacionamentos:**
- Many-to-Many com Product

#### Order (tb_order)
Representa um pedido realizado por um usuário.

| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | Long | Identificador único (PK) |
| moment | Instant | Data/hora do pedido |
| orderStatus | Integer | Status do pedido (enum) |
| client_id | Long | FK para usuário (cliente) |

**Relacionamentos:**
- Many-to-One com User
- One-to-Many com OrderItem
- One-to-One com Payment

#### OrderItem (tb_order_item)
Representa um item dentro de um pedido (chave composta).

| Campo | Tipo | Descrição |
|-------|------|-----------|
| order_id | Long | FK para Order |
| product_id | Long | FK para Product |
| quantity | Integer | Quantidade |
| price | Double | Preço unitário no momento |

#### Payment (tb_payment)
Representa o pagamento de um pedido.

**Relacionamentos:**
- One-to-One com Order

### Diagrama de Relacionamentos (ER)

```
User (1) ──────────────── (N) Order
                              ↓ (1)
                          Payment (1)
                              
Order (1) ──────────────── (N) OrderItem (N) ──────────────── (1) Product
                                                                    ↑
                                                                    │
                                                                  (N)
                                                              Category (N)
```

## 🚀 Endpoints da API

### Usuários (User Resource)

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/users` | Listar todos os usuários |
| GET | `/users/{id}` | Obter usuário por ID |
| POST | `/users` | Criar novo usuário |
| PUT | `/users/{id}` | Atualizar usuário |
| DELETE | `/users/{id}` | Deletar usuário |

### Produtos (Product Resource)

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/products` | Listar todos os produtos |
| GET | `/products/{id}` | Obter produto por ID |
| POST | `/products` | Criar novo produto |
| PUT | `/products/{id}` | Atualizar produto |
| DELETE | `/products/{id}` | Deletar produto |

### Categorias (Category Resource)

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/categories` | Listar todas as categorias |
| GET | `/categories/{id}` | Obter categoria por ID |
| POST | `/categories` | Criar nova categoria |
| PUT | `/categories/{id}` | Atualizar categoria |
| DELETE | `/categories/{id}` | Deletar categoria |

### Pedidos (Order Resource)

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/orders` | Listar todos os pedidos |
| GET | `/orders/{id}` | Obter pedido por ID |
| POST | `/orders` | Criar novo pedido |
| PUT | `/orders/{id}` | Atualizar pedido |
| DELETE | `/orders/{id}` | Deletar pedido |

## ⚙️ Configuração e Instalação

### Pré-requisitos
- Java 25+
- Maven 3.8+
- Git

### Passos para Instalação

1. **Clone o repositório**
```bash
git clone https://github.com/seu-usuario/ecommerce-api.git
cd ecommerce-api
```

2. **Configure o banco de dados**

Para desenvolvimento com **H2** (padrão):
- Nenhuma configuração adicional necessária
- Console H2 disponível em: `http://localhost:8080/h2-console`

Para produção com **PostgreSQL**:
Edite `src/main/resources/application.properties`:
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/ecommerce_db
spring.datasource.username=seu_usuario
spring.datasource.password=sua_senha
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQL10Dialect
```

3. **Compile e execute**
```bash
# Compilar o projeto
mvn clean install

# Executar a aplicação
mvn spring-boot:run
```

A API estará disponível em: `http://localhost:8080`

## 📝 Exemplo de Uso

### Criar um Usuário
```bash
curl -X POST http://localhost:8080/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "João Silva",
    "email": "joao@example.com",
    "phone": "11999999999",
    "password": "senha123"
  }'
```

### Listar Todos os Usuários
```bash
curl -X GET http://localhost:8080/users
```

### Obter Usuário por ID
```bash
curl -X GET http://localhost:8080/users/1
```

### Atualizar Usuário
```bash
curl -X PUT http://localhost:8080/users/1 \
  -H "Content-Type: application/json" \
  -d '{
    "name": "João Silva Santos",
    "email": "joao.silva@example.com",
    "phone": "11988888888"
  }'
```

### Deletar Usuário
```bash
curl -X DELETE http://localhost:8080/users/1
```

## 🎓 Conceitos Demonstrados

### Spring Framework
- ✅ Dependency Injection (DI)
- ✅ Inversion of Control (IoC)
- ✅ Component Scanning
- ✅ Auto-configuration

### Camada de Persistência
- ✅ JPA (Java Persistence API)
- ✅ Hibernate ORM
- ✅ JPQL (Java Persistence Query Language)
- ✅ Transações gerenciadas
- ✅ Lazy Loading e Eager Loading

### REST API
- ✅ Endpoints RESTful
- ✅ HTTP Methods (GET, POST, PUT, DELETE)
- ✅ JSON serialization/deserialization
- ✅ Status HTTP apropriados

### Tratamento de Erros
- ✅ Exception Handling
- ✅ Custom Exceptions
- ✅ ResourceNotFoundException
- ✅ DatabaseException
- ✅ Respostas de erro estruturadas

### Padrões de Design
- ✅ Repository Pattern
- ✅ Service Layer Pattern
- ✅ DTO (Data Transfer Object)
- ✅ Factory Pattern

## 📂 Estrutura de Diretórios

```
course/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/educandoweb/course/
│   │   │       ├── CourseApplication.java      # Classe principal
│   │   │       ├── config/                     # Configurações
│   │   │       ├── entities/                   # Entidades JPA
│   │   │       │   ├── User.java
│   │   │       │   ├── Product.java
│   │   │       │   ├── Category.java
│   │   │       │   ├── Order.java
│   │   │       │   ├── OrderItem.java
│   │   │       │   ├── Payment.java
│   │   │       │   └── enums/
│   │   │       ├── repositories/               # Data Access Layer
│   │   │       │   ├── UserRepository.java
│   │   │       │   ├── ProductRepository.java
│   │   │       │   ├── CategoryRepository.java
│   │   │       │   └── OrderRepository.java
│   │   │       ├── resources/                  # REST Controllers
│   │   │       │   ├── UserResource.java
│   │   │       │   ├── ProductResource.java
│   │   │       │   ├── CategoryResource.java
│   │   │       │   ├── OrderResource.java
│   │   │       │   └── exceptions/
│   │   │       └── services/                   # Business Logic Layer
│   │   │           ├── UserService.java
│   │   │           ├── ProductService.java
│   │   │           ├── CategoryService.java
│   │   │           ├── OrderService.java
│   │   │           └── exceptions/
│   │   └── resources/
│   │       ├── application.properties          # Config produção
│   │       └── application-test.properties     # Config teste
│   └── test/
│       └── java/...                            # Testes unitários
├── pom.xml                                     # Maven configuration
└── README.md
```

## 🧪 Testes

Execute os testes da aplicação:
```bash
mvn test
```

## 🔐 Boas Práticas Implementadas

- ✅ **Separação de responsabilidades** - Camadas bem definidas (Controller → Service → Repository)
- ✅ **Injeção de Dependência** - Uso do `@Autowired` para loosely-coupled components
- ✅ **Tratamento de Exceções** - Custom exceptions e global exception handler
- ✅ **Validação de Dados** - Validações na camada de serviço
- ✅ **Lazy Loading** - Evita carregamentos desnecessários
- ✅ **JSON Filtering** - Uso de `@JsonIgnore` para evitar ciclos de referência
- ✅ **Status HTTP Apropriados** - Respostas corretas para cada situação
- ✅ **Documentação de Código** - Classes e métodos bem comentados

## 📚 Aprendizados e Competências

Este projeto demonstra proficiência em:

- **Backend Development** com Spring Boot
- **Banco de Dados** e ORM com JPA/Hibernate
- **REST API Design** e HTTP
- **Java** e Programação Orientada a Objetos
- **Git** e versionamento
- **Maven** e build automation
- **Padrões de Design** e Arquitetura de Software
- **Tratamento de Erros** e Logging
- **SQL** e Relacionamentos de Dados

## 🚀 Melhorias Futuras

- [ ] Autenticação e Autorização (JWT)
- [ ] Rate Limiting
- [ ] Paginação de resultados
- [ ] Filtros avançados
- [ ] Documentação Swagger/OpenAPI
- [ ] Testes de integração com TestContainers
- [ ] Deploy com Docker
- [ ] CI/CD Pipeline
- [ ] Caching com Redis
- [ ] Validação com Bean Validation

## 📄 Licença

Este projeto está licenciado sob a MIT License - veja o arquivo LICENSE para detalhes.

## 👤 Autor

**Seu Nome**
- GitHub: [@seu-usuario](https://github.com/seu-usuario)
- LinkedIn: [linkedin.com/in/seu-perfil](https://linkedin.com/in/seu-perfil)
- Email: seu.email@example.com

## 🤝 Contribuições

Contribuições são bem-vindas! Sinta-se à vontade para fazer um fork do projeto, criar uma branch para sua feature e enviar um pull request.

## 📞 Suporte

Se tiver dúvidas ou encontrar problemas, abra uma issue no repositório do GitHub.

---

**Desenvolvido com ❤️ como projeto educacional e de portfolio**

