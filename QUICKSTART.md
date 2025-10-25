# 🚀 Guia Rápido de Execução

## ⚡ Início Rápido (5 minutos)

### Pré-requisitos
- ✅ Java 17 ou superior
- ✅ Maven 3.8+
- ✅ Node.js 18+ e npm
- ✅ Git

### Passo 1: Clonar o Repositório
```bash
git clone <repository-url>
cd bip-teste-integrado-main
```

### Passo 2: Executar Backend

```bash
# Terminal 1 - Backend
cd backend-module

# Compilar e executar
mvn clean spring-boot:run
```

Aguarde a mensagem: `Started BackendApplication in X seconds`

### Passo 3: Executar Frontend

```bash
# Terminal 2 - Frontend
cd frontend

# Instalar dependências (primeira vez apenas)
npm install

# Executar aplicação
npm start
```

Aguarde a mensagem: `Angular Live Development Server is listening on localhost:4200`

### Passo 4: Acessar a Aplicação

Abra seu navegador em: **http://localhost:4200**

## 🎯 URLs Importantes

| Serviço | URL | Descrição |
|---------|-----|-----------|
| **Frontend** | http://localhost:4200 | Interface do usuário |
| **Backend API** | http://localhost:8080/api/v1/beneficios | REST API |
| **Swagger UI** | http://localhost:8080/swagger-ui.html | Documentação interativa |
| **H2 Console** | http://localhost:8080/h2-console | Console do banco de dados |

### Credenciais H2 Console
- **JDBC URL**: `jdbc:h2:mem:beneficiodb`
- **Username**: `sa`
- **Password**: (deixe em branco)

## 🧪 Executar Testes

### Testes Backend
```bash
cd backend-module
mvn test
```

### Testes EJB
```bash
cd ejb-module
mvn test
```

### Testes Frontend
```bash
cd frontend
npm test
```

## 📝 Testar a API Manualmente

### 1. Listar Benefícios
```bash
curl http://localhost:8080/api/v1/beneficios
```

### 2. Criar Benefício
```bash
curl -X POST http://localhost:8080/api/v1/beneficios \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "Beneficio Teste",
    "descricao": "Descrição do teste",
    "valor": 500.00,
    "ativo": true
  }'
```

### 3. Transferir Valor
```bash
curl -X POST http://localhost:8080/api/v1/beneficios/transferir \
  -H "Content-Type: application/json" \
  -d '{
    "fromId": 1,
    "toId": 2,
    "amount": 100.00
  }'
```

## 🐛 Solução de Problemas

### Erro: "Port 8080 already in use"
```bash
# Windows
netstat -ano | findstr :8080
taskkill /PID <PID> /F

# Linux/Mac
lsof -ti:8080 | xargs kill -9
```

### Erro: "Port 4200 already in use"
```bash
# Matar processo na porta 4200
# Windows
netstat -ano | findstr :4200
taskkill /PID <PID> /F

# Linux/Mac
lsof -ti:4200 | xargs kill -9
```

### Erro: "Maven dependencies not found"
```bash
cd backend-module
mvn clean install -U
```

### Erro: "npm dependencies not found"
```bash
cd frontend
rm -rf node_modules package-lock.json
npm install
```

## 🎨 Funcionalidades Principais

### No Frontend você pode:
1. ✅ **Criar Benefícios**: Preencha o formulário e clique em "Criar Benefício"
2. ✅ **Listar Benefícios**: Veja todos os benefícios cadastrados
3. ✅ **Buscar Benefícios**: Use a barra de busca para filtrar por nome
4. ✅ **Transferir Valores**: Selecione origem, destino e valor
5. ✅ **Inativar Benefícios**: Clique no botão "Inativar"

### Validações Automáticas:
- ❌ Não permite transferir para o mesmo benefício
- ❌ Não permite valores negativos
- ❌ Não permite transferir mais que o saldo disponível
- ❌ Não permite transferir de/para benefícios inativos

## 📊 Dados de Exemplo

O sistema já vem com 2 benefícios cadastrados:

| ID | Nome | Descrição | Valor | Status |
|----|------|-----------|-------|--------|
| 1 | Beneficio A | Descrição A | R$ 1.000,00 | Ativo |
| 2 | Beneficio B | Descrição B | R$ 500,00 | Ativo |

## 🔄 Resetar Banco de Dados

O banco H2 é em memória, então basta reiniciar o backend:

```bash
# Parar o backend (Ctrl+C)
# Iniciar novamente
mvn spring-boot:run
```

## 📚 Próximos Passos

1. ✅ Explore a documentação Swagger em http://localhost:8080/swagger-ui.html
2. ✅ Teste diferentes cenários de transferência
3. ✅ Verifique os logs do backend para entender o fluxo
4. ✅ Acesse o H2 Console para ver os dados no banco
5. ✅ Execute os testes para ver a cobertura

## 💡 Dicas

- Use o **Swagger UI** para testar a API de forma interativa
- Abra o **Console do Navegador** (F12) para ver logs do frontend
- Use o **H2 Console** para executar queries SQL diretamente
- Os logs do backend aparecem no terminal onde você executou `mvn spring-boot:run`

## 🎓 Arquitetura Rápida

```
Frontend (Angular) → Backend (Spring Boot) → EJB Module → Database (H2)
     :4200                  :8080              (JPA)        (Memory)
```

## ✅ Checklist de Verificação

Antes de considerar que tudo está funcionando:

- [ ] Backend iniciou sem erros
- [ ] Frontend iniciou sem erros
- [ ] Consegue acessar http://localhost:4200
- [ ] Consegue ver a lista de benefícios
- [ ] Consegue criar um novo benefício
- [ ] Consegue fazer uma transferência
- [ ] Swagger UI está acessível
- [ ] H2 Console está acessível

Se todos os itens estão marcados, **parabéns! 🎉** O sistema está funcionando perfeitamente!

## 📞 Suporte

Para mais detalhes, consulte:
- **README.md** - Documentação completa
- **SOLUTION.md** - Detalhes da solução implementada
- **Swagger UI** - Documentação da API
