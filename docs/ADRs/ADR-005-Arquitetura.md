# ADR 5: Arquitetura do Sistema

## Status
Aceito

## Contexto
Precisamos definir uma arquitetura de sistema que seja clara, didática e adequada para um projeto CRUD básico, permitindo que alunos iniciantes entendam o fluxo de dados e a separação de responsabilidades.

## Decisão
Implementaremos uma arquitetura cliente-servidor em três camadas com frontend React, API RESTful e banco de dados SQLite.

## Justificativa
- **Separação de Responsabilidades**: A arquitetura em camadas proporciona uma clara divisão de responsabilidades entre frontend, backend e persistência.
- **Padrão API RESTful**: Utiliza métodos HTTP standard (GET, POST, PUT, DELETE) que são amplamente adotados na indústria.
- **Simplicidade Conceitual**: O modelo cliente-servidor é intuitivo e facilmente visualizável para alunos iniciantes.
- **Escalabilidade Educacional**: Proporciona fundamentos que podem ser expandidos para arquiteturas mais complexas.

## Detalhes da Arquitetura
1. **Camada de Apresentação (Frontend)**
   - React com TypeScript
   - Componentes para interface de usuário
   - Hooks para gerenciamento de estado local
   - Axios para comunicação com a API

2. **Camada de Serviço (Backend)**
   - Node.js com Express
   - Controladores para gerenciar requisições HTTP
   - Serviços para lógica de negócios
   - Prisma como ORM para acesso ao banco de dados

3. **Camada de Dados**
   - SQLite como banco de dados relacional
   - Schema Prisma para definição do modelo de dados
   - Migrações para controle de versão do banco

## Fluxo de Dados
1. O usuário interage com a interface React
2. O frontend faz requisições HTTP para endpoints da API
3. Os controladores no backend processam as requisições
4. Os serviços implementam a lógica de negócios
5. O ORM Prisma traduz operações para consultas SQL
6. O banco SQLite armazena ou retorna dados
7. A resposta percorre o caminho inverso até o usuário

## Consequências
- **Positivas**:
  - Modelo mental claro do sistema
  - Separação que facilita o desenvolvimento paralelo
  - Introdução a padrões de arquitetura utilizados em aplicações reais
  - Base para explorar arquiteturas mais avançadas no futuro

- **Negativas**:
  - Mais complexa que uma aplicação monolítica simples
  - Overhead de comunicação entre camadas
  - Necessidade de configurar CORS e gerenciar estados entre cliente e servidor

## Alternativas Consideradas
- **Aplicação Monolítica**: Mais simples, mas não demonstraria a separação frontend/backend comum em projetos modernos.
- **Backend Serverless**: Eliminaria a necessidade de um servidor tradicional, mas adicionaria complexidade conceitual.
- **GraphQL**: Alternativa mais flexível para API, mas com curva de aprendizado maior para iniciantes.
- **Arquitetura MVC Tradicional**: Mais estruturada, mas potencialmente mais rígida para uma aplicação React moderna.
