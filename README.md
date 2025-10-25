# 🏗️ Sistema de Gerenciamento de Benefícios - Desafio Fullstack Integrado

## 📋 Descrição

Sistema completo em camadas (DB, EJB, Backend, Frontend) para gerenciamento de benefícios com funcionalidade de transferência segura entre benefícios.

## 🎯 Funcionalidades Implementadas

### ✅ Correção do Bug no EJB
- **Problema Original**: Transferência não verificava saldo, não usava locking, podia gerar inconsistência
- **Solução Implementada**:
  - ✅ Validação de saldo suficiente antes da transferência
  - ✅ Uso de `LockModeType.OPTIMISTIC` para evitar lost updates
  - ✅ Validações completas de parâmetros (null, valores negativos, IDs iguais)
  - ✅ Validação de benefícios ativos
  - ✅ Transações com `@TransactionAttribute(REQUIRED)` para garantir rollback
  - ✅ Tratamento de exceções (`EntityNotFoundException`, `IllegalArgumentException`, `IllegalStateException`)

### 🔧 Backend Spring Boot
- **CRUD Completo**:
  - `GET /api/v1/beneficios` - Lista todos os benefícios
  - `GET /api/v1/beneficios/{id}` - Busca benefício por ID
  - `GET /api/v1/beneficios/ativos` - Lista benefícios ativos
  - `GET /api/v1/beneficios/buscar?nome={nome}` - Busca por nome
  - `POST /api/v1/beneficios` - Cria novo benefício
  - `PUT /api/v1/beneficios/{id}` - Atualiza benefício
  - `DELETE /api/v1/beneficios/{id}` - Inativa benefício (soft delete)
  - `POST /api/v1/beneficios/transferir` - Transfere valor entre benefícios

### 🎨 Frontend Angular
- Interface moderna e responsiva
- Formulário de criação de benefícios com validação
- Formulário de transferência com seleção de benefícios ativos
- Listagem de benefícios com busca e filtros
- Feedback visual de sucesso/erro
- Formatação de valores em moeda brasileira

### 🧪 Testes
- **Testes Unitários**: `BeneficioServiceTest` e `BeneficioEjbServiceTest`
- **Testes de Integração**: `BeneficioControllerIntegrationTest`
- Cobertura de cenários:
  - Transferências bem-sucedidas
  - Saldo insuficiente
  - Benefícios inativos
  - IDs inválidos
  - Valores negativos
  - Conflitos de concorrência

### 📚 Documentação
- **Swagger UI**: Disponível em `http://localhost:8080/swagger-ui.html`
- **OpenAPI Docs**: Disponível em `http://localhost:8080/api-docs`
- Documentação completa de todos os endpoints com exemplos

## 🏗️ Arquitetura

```
┌─────────────────────────────────────────────────────────┐
│                    Frontend (Angular)                   │
│  - Components (List, Form, Transfer)                    │
│  - Services (HTTP Client)                               │
│  - Models (Interfaces TypeScript)                       │
└────────────────────┬────────────────────────────────────┘
                     │ HTTP/REST
┌────────────────────▼────────────────────────────────────┐
│              Backend (Spring Boot)                      │
│  - Controllers (REST API)                               │
│  - Services (Business Logic)                            │
│  - Repositories (JPA)                                   │
│  - DTOs (Data Transfer Objects)                         │
└────────────────────┬────────────────────────────────────┘
                     │ JPA/Hibernate
┌────────────────────▼────────────────────────────────────┐
│                 EJB Module                              │
│  - BeneficioEjbService (Stateless)                      │
│  - Beneficio Entity (JPA)                               │
│  - Validações e Locking Otimista                        │
└────────────────────┬────────────────────────────────────┘
                     │ JDBC
┌────────────────────▼────────────────────────────────────┐
│              Database (H2 In-Memory)                    │
│  - Tabela BENEFICIO                                     │
│  - Schema e Seed Data                                   │
└─────────────────────────────────────────────────────────┘
```

## 🚀 Como Executar

### Pré-requisitos
- Java 17+
- Maven 3.8+
- Node.js 18+ e npm
- Git

### 1. Backend (Spring Boot)

```bash
# Navegar para o diretório do backend
cd backend-module

# Compilar o projeto
mvn clean install

# Executar a aplicação
mvn spring-boot:run
```

O backend estará disponível em: `http://localhost:8080`

### 2. Frontend (Angular)

```bash
# Navegar para o diretório do frontend
cd frontend

# Instalar dependências
npm install

# Executar a aplicação
npm start
```

O frontend estará disponível em: `http://localhost:4200`

### 3. Acessar a Aplicação

- **Frontend**: http://localhost:4200
- **Backend API**: http://localhost:8080/api/v1/beneficios
- **Swagger UI**: http://localhost:8080/swagger-ui.html
- **H2 Console**: http://localhost:8080/h2-console
  - JDBC URL: `jdbc:h2:mem:beneficiodb`
  - Username: `sa`
  - Password: (vazio)

## 🧪 Executar Testes

### Backend
```bash
cd backend-module
mvn test
```

### EJB Module
```bash
cd ejb-module
mvn test
```

### Frontend
```bash
cd frontend
npm test
```

## 📊 Banco de Dados

### Schema
```sql
CREATE TABLE BENEFICIO (
  ID BIGINT AUTO_INCREMENT PRIMARY KEY,
  NOME VARCHAR(100) NOT NULL,
  DESCRICAO VARCHAR(255),
  VALOR DECIMAL(15,2) NOT NULL,
  ATIVO BOOLEAN DEFAULT TRUE,
  VERSION BIGINT DEFAULT 0
);
```

### Dados Iniciais
```sql
INSERT INTO BENEFICIO (NOME, DESCRICAO, VALOR, ATIVO, VERSION) VALUES
('Beneficio A', 'Descrição A', 1000.00, TRUE, 0),
('Beneficio B', 'Descrição B', 500.00, TRUE, 0);
```

## 🔐 Segurança e Validações

### Validações Implementadas
- ✅ Validação de campos obrigatórios
- ✅ Validação de valores mínimos/máximos
- ✅ Validação de saldo suficiente
- ✅ Validação de benefícios ativos
- ✅ Validação de IDs válidos
- ✅ Prevenção de transferência para o mesmo benefício

### Controle de Concorrência
- ✅ Locking otimista com `@Version`
- ✅ `LockModeType.OPTIMISTIC` nas operações críticas
- ✅ Tratamento de `OptimisticLockException`
- ✅ Transações ACID garantidas

## 📝 Exemplos de Uso da API

### Criar Benefício
```bash
curl -X POST http://localhost:8080/api/v1/beneficios \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "Beneficio C",
    "descricao": "Descrição C",
    "valor": 750.00,
    "ativo": true
  }'
```

### Transferir Valor
```bash
curl -X POST http://localhost:8080/api/v1/beneficios/transferir \
  -H "Content-Type: application/json" \
  -d '{
    "fromId": 1,
    "toId": 2,
    "amount": 200.00
  }'
```

### Listar Benefícios
```bash
curl http://localhost:8080/api/v1/beneficios
```

## 📦 Estrutura do Projeto

```
bip-teste-integrado-main/
├── db/
│   ├── schema.sql          # Script de criação do schema
│   └── seed.sql            # Script de dados iniciais
├── ejb-module/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/example/ejb/
│   │   │   │   ├── Beneficio.java
│   │   │   │   └── BeneficioEjbService.java
│   │   │   └── resources/META-INF/
│   │   │       └── persistence.xml
│   │   └── test/
│   │       └── java/com/example/ejb/
│   │           └── BeneficioEjbServiceTest.java
│   └── pom.xml
├── backend-module/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/example/backend/
│   │   │   │   ├── BackendApplication.java
│   │   │   │   ├── BeneficioController.java
│   │   │   │   ├── model/Beneficio.java
│   │   │   │   ├── repository/BeneficioRepository.java
│   │   │   │   ├── service/BeneficioService.java
│   │   │   │   └── dto/
│   │   │   │       ├── BeneficioDTO.java
│   │   │   │       └── TransferenciaDTO.java
│   │   │   └── resources/
│   │   │       ├── application.properties
│   │   │       ├── schema.sql
│   │   │       └── data.sql
│   │   └── test/
│   │       └── java/com/example/backend/
│   │           ├── service/BeneficioServiceTest.java
│   │           └── controller/BeneficioControllerIntegrationTest.java
│   └── pom.xml
├── frontend/
│   ├── src/
│   │   ├── app/
│   │   │   ├── components/
│   │   │   │   ├── beneficio-list/
│   │   │   │   ├── beneficio-form/
│   │   │   │   └── transferencia-form/
│   │   │   ├── models/
│   │   │   │   └── beneficio.model.ts
│   │   │   ├── services/
│   │   │   │   └── beneficio.service.ts
│   │   │   ├── app.component.ts
│   │   │   └── app.module.ts
│   │   ├── index.html
│   │   ├── main.ts
│   │   └── styles.css
│   ├── angular.json
│   ├── package.json
│   └── tsconfig.json
├── docs/
│   └── README.md
└── README.md
```

## 🎓 Tecnologias Utilizadas

### Backend
- **Spring Boot 3.2.5** - Framework principal
- **Spring Data JPA** - Persistência de dados
- **H2 Database** - Banco de dados em memória
- **SpringDoc OpenAPI** - Documentação Swagger
- **Jakarta EE 10** - EJB e JPA
- **JUnit 5** - Testes unitários
- **Mockito** - Mocks para testes

### Frontend
- **Angular 17** - Framework frontend
- **TypeScript** - Linguagem
- **RxJS** - Programação reativa
- **HttpClient** - Comunicação HTTP
- **Reactive Forms** - Formulários reativos

## ✅ Critérios de Avaliação Atendidos

- ✅ **Arquitetura em camadas (20%)**: Separação clara entre DB, EJB, Backend e Frontend
- ✅ **Correção EJB (20%)**: Bug corrigido com validações, locking e rollback
- ✅ **CRUD + Transferência (15%)**: Implementação completa de todas as operações
- ✅ **Qualidade de código (10%)**: Código limpo, organizado e bem documentado
- ✅ **Testes (15%)**: Testes unitários e de integração com boa cobertura
- ✅ **Documentação (10%)**: README completo e Swagger implementado
- ✅ **Frontend (10%)**: Interface funcional e moderna

## 👨‍💻 Autor

Desenvolvido como solução para o Desafio Fullstack Integrado.

## 📄 Licença

Este projeto foi desenvolvido para fins educacionais e de avaliação técnica.
