# 🐾 Projeto Pet — API de Gestão da Jornada do Pet

> API REST desenvolvida com **Java 21 + Spring Boot 4.0** para gerenciar a jornada completa do pet,
> incluindo tutores, pets, tipos de cuidado e eventos de cuidado.
> Projeto acadêmico — FIAP 2026 | Disciplina: Java Advanced

---

## 👥 Equipe

| Nome | RM |
|---|---|
| Yasmin Nathalin Miranda dos Santos | RM561365 |
| Riquelme Nascimento | RM565468 |
| Enzo Franchin de Souza | RM565677 |
| Lucas da Silva Lima | RM562118 |

---

## 🎯 Objetivo da Aplicação

O sistema foi desenvolvido para resolver o problema da **falta de controle e organização dos cuidados veterinários dos pets**. Muitos tutores perdem prazos de vacinas, vermifugações e consultas por não terem uma forma centralizada de acompanhar a jornada do seu animal.

A API permite:

- Cadastrar tutores e seus respectivos pets
- Definir tipos de cuidado (vacinas, banhos, consultas, antipulgas etc.) com prioridade e intervalo de repetição
- Registrar e acompanhar eventos de cuidado por pet
- **Listar eventos atrasados** — identificar quais cuidados já passaram do prazo
- **Listar próximos cuidados por pet** — planejar os próximos agendamentos
- **Buscar pets por espécie, raça ou nome** — localização rápida
- **Filtrar eventos por status e período** — visão completa da agenda de cuidados

---

## 🏗️ Arquitetura da Aplicação

```
br.com.fiap.projeto_pet
├── config/         → Configuração do Swagger/OpenAPI
├── control/        → Controllers REST (endpoints da API)
├── dto/            → Data Transfer Objects (entrada/saída)
├── model/          → Entidades JPA e Enums
├── projection/     → Interfaces de projeção (SQL nativo)
├── repository/     → Repositories JPA com JPQL e SQL nativo
├── security/       → JWT, filtros e configuração de segurança
├── service/        → Serviços de Cache e Paginação
└── validation/     → Tratamento global de exceções
```

---

## 🗂️ Entidades do Domínio

| Entidade | Descrição |
|---|---|
| `Tutor` | Responsável pelo pet (nome, CPF, e-mail, telefone) |
| `Pet` | Animal cadastrado vinculado a um tutor (espécie, raça, peso, data de nascimento) |
| `TipoCuidado` | Tipo de cuidado veterinário (vacina, banho, consulta) com prioridade e intervalo |
| `EventoCuidado` | Registro de um cuidado agendado ou realizado para um pet específico |
| `Pessoa` | Pessoa física vinculada ao usuário do sistema |
| `Usuario` | Usuário de acesso à API com autenticação JWT |

---

## ⚙️ Tecnologias Utilizadas

- **Java 21**
- **Spring Boot 4.0.3**
- **Spring Data JPA + Hibernate**
- **Spring Security + JWT (jjwt 0.12.6)**
- **Oracle Database** (FIAP — `oracle.fiap.com.br`)
- **Bean Validation (Jakarta)**
- **SpringDoc OpenAPI 3 (Swagger UI)**
- **Spring Cache (@Cacheable / @CacheEvict)**
- **Maven**

---

## 🔧 Configuração e Execução

### Pré-requisitos

- Java 21 instalado
- Eclipse IDE for Enterprise Java (ou IntelliJ)
- Acesso à rede/VPN da FIAP
- Maven (embutido no Eclipse)

### Passos para rodar

**1. Clonar o repositório**
```bash
git clone https://github.com/SEU_USUARIO/projeto_pet.git
```

**2. Importar no Eclipse**
```
File → Import → Maven → Existing Maven Projects → selecionar a pasta projeto_pet
```

**3. Aguardar o Maven baixar as dependências**

Acompanhe o progresso na barra inferior do Eclipse (pode levar alguns minutos na primeira vez).

**4. Verificar o `application.properties`**

```properties
spring.datasource.url=jdbc:oracle:thin:@oracle.fiap.com.br:1521:ORCL
spring.datasource.username=SEU_RM
spring.datasource.password=SUA_SENHA
spring.jpa.hibernate.ddl-auto=create
```

> ⚠️ O `ddl-auto=create` cria as tabelas automaticamente na primeira execução.
> Após confirmar o funcionamento, altere para `update` para preservar os dados.

**5. Executar a aplicação**

```
Clique com botão direito em ProjetoPetApplication.java → Run As → Spring Boot App
```

**6. Confirmar que subiu corretamente**

No console do Eclipse, aguarde a mensagem:
```
Started ProjetoPetApplication in X.XXX seconds
```

---

## 📖 Documentação da API (Swagger)

Após iniciar a aplicação, acesse:

```
http://localhost:8080/swagger-ui/index.html
```

> 🔐 **As credenciais de acesso (usuário e senha para gerar o token JWT) estão disponíveis na primeira página da documentação Swagger**, na descrição da API.
>
> **Login padrão:** usuário `RM1` — senha `senha`

---

## 🔐 Autenticação

A API utiliza **JWT (JSON Web Token)**. Para acessar os endpoints protegidos:

**1. Gerar o token**

```http
POST /autenticacao/login?usuario=RM1&senha=senha&duracao=60
```

**2. Copiar o token retornado**

```
eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJSTTEi...
```

**3. Autorizar no Swagger**

Clique no botão **Authorize** (cadeado) no topo do Swagger → cole o token → clique em **Authorize**.

**4. Autorizar no Postman**

Em cada requisição, vá em **Authorization → Bearer Token** e cole o token.

---

## 🚀 Endpoints Principais

### Autenticação
| Método | Endpoint | Descrição |
|---|---|---|
| POST | `/autenticacao/login` | Gera token JWT |

### Tutores
| Método | Endpoint | Descrição |
|---|---|---|
| GET | `/tutores/todos` | Lista todos os tutores |
| GET | `/tutores/{id}` | Busca tutor por ID |
| POST | `/tutores/inserir` | Cadastra novo tutor |
| PUT | `/tutores/{id}` | Atualiza tutor |
| DELETE | `/tutores/{id}` | Remove tutor |

### Pets
| Método | Endpoint | Descrição |
|---|---|---|
| GET | `/pets/todos` | Lista todos os pets (cache) |
| GET | `/pets/paginados` | Lista paginada com ordenação |
| GET | `/pets/{id}` | Busca pet por ID |
| GET | `/pets/especie?especie=CACHORRO` | Busca por espécie (JPQL) |
| GET | `/pets/raca?raca=Labrador` | Busca por raça |
| GET | `/pets/substring?substring=rex` | Busca por nome, raça ou tutor |
| POST | `/pets/inserir` | Cadastra novo pet |
| PUT | `/pets/{id}` | Atualiza pet |
| DELETE | `/pets/{id}` | Remove pet |

### Tipos de Cuidado
| Método | Endpoint | Descrição |
|---|---|---|
| GET | `/tipos_cuidado/todos` | Lista todos (cache) |
| GET | `/tipos_cuidado/{id}` | Busca por ID |
| GET | `/tipos_cuidado/prioridade?prioridade=ALTA` | Busca por prioridade (JPQL) |
| POST | `/tipos_cuidado/inserir` | Cadastra novo tipo |
| PUT | `/tipos_cuidado/{id}` | Atualiza tipo |
| DELETE | `/tipos_cuidado/{id}` | Remove tipo |

### Eventos de Cuidado ⭐ (além do CRUD)
| Método | Endpoint | Descrição |
|---|---|---|
| GET | `/eventos/todos` | Lista todos os eventos (cache) |
| GET | `/eventos/paginados` | Lista paginada com ordenação |
| GET | `/eventos/{id}` | Busca evento por ID |
| GET | `/eventos/atrasados` | ⚠️ Lista eventos com status ATRASADO |
| GET | `/eventos/proximos_cuidados?petId=1` | 📅 Próximos cuidados de um pet |
| GET | `/eventos/status?status=PENDENTE` | Filtra por status (JPQL) |
| GET | `/eventos/status_otimizado?status=PENDENTE` | Filtra por status com cache |
| GET | `/eventos/periodo?dataInicio=2025-01-01&dataFim=2025-12-31` | Filtra por período |
| POST | `/eventos/inserir` | Registra novo evento |
| PUT | `/eventos/{id}` | Atualiza evento |
| DELETE | `/eventos/{id}` | Remove evento |

---

## 🧪 Como Testar

### Opção 1 — Swagger UI (mais fácil)

1. Acesse `http://localhost:8080/swagger-ui/index.html`
2. Execute `POST /autenticacao/login` com `usuario=RM1` e `senha=senha`
3. Copie o token retornado
4. Clique em **Authorize** → cole o token → confirme
5. Teste os endpoints diretamente pela interface

### Opção 2 — Postman

1. Abra o Postman
2. Clique em **Import** e selecione o arquivo:
   ```
   documentos/projeto_pet_postman_collection.json
   ```
3. Execute primeiro o request **01 - Autenticação → Login**
4. Copie o token e configure a variável `{{token}}` na collection
5. Execute os demais requests em sequência

### Verificar dados no banco Oracle

Conecte no SQL Developer com suas credenciais Oracle da FIAP.

Queries de verificação:
```sql
SELECT * FROM tutor;
SELECT * FROM pet;
SELECT * FROM tipo_cuidado;
SELECT * FROM evento_cuidado;

-- Eventos atrasados
SELECT p.nome pet, tc.nome cuidado, e.data_prevista, e.status
FROM evento_cuidado e
JOIN pet p ON e.fk_pet = p.id
JOIN tipo_cuidado tc ON e.fk_tipo_cuidado = tc.id
WHERE e.status = 'ATRASADO'
ORDER BY e.data_prevista;
```

---

## ✅ Requisitos Técnicos Atendidos

| Requisito | Implementação |
|---|---|
| ✅ Bean Validation | `@NotEmpty`, `@Email`, `@Size`, `@DecimalMin/Max`, `@PastOrPresent` nas entidades |
| ✅ Paginação | `PageRequest` nos endpoints `/paginados` |
| ✅ Ordenação | Parâmetro `ordenarPor` nos endpoints paginados |
| ✅ Busca com parâmetros | `/especie`, `/raca`, `/substring`, `/status`, `/periodo` |
| ✅ Cache | `@Cacheable` e `@CacheEvict` em PetCachingService e EventoCachingService |
| ✅ Tratamento de exceções | `GerenciadorValidacoes` com `@RestControllerAdvice` |
| ✅ DTOs | `PetDTO` e `EventoCuidadoDTO` com conversão nas services |
| ✅ Swagger | SpringDoc OpenAPI com `@Operation` em todos os endpoints |
| ✅ JWT / Segurança | `JWTUtil`, `JWTAuthFilter`, `SecurityConfig` |
| ✅ JPQL | Queries em `PetRepository`, `EventoCuidadoRepository`, `TipoCuidadoRepository` |
| ✅ SQL Nativo + Projections | `PetProjection`, `EventoProjection` com native queries |
| ✅ Além do CRUD | Eventos atrasados, próximos cuidados, busca por período |

---

## 📁 Estrutura de Arquivos do Repositório

```
projeto_pet/
├── src/
│   ├── main/
│   │   ├── java/br/com/fiap/projeto_pet/
│   │   │   ├── ProjetoPetApplication.java
│   │   │   ├── config/
│   │   │   ├── control/
│   │   │   ├── dto/
│   │   │   ├── model/
│   │   │   ├── projection/
│   │   │   ├── repository/
│   │   │   ├── security/
│   │   │   ├── service/
│   │   │   └── validation/
│   │   └── resources/
│   │       ├── application.properties
│   │       └── import.sql
│   └── test/
├── documentos/
│   └── projeto_pet_postman_collection.json
├── pom.xml
└── README.md
```

---

## 📄 Licença

Projeto acadêmico desenvolvido para a disciplina de **Java Advanced — FIAP 2026**.
