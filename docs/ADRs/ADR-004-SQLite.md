# ADR 4: Escolha do SQLite como Banco de Dados

## Status
Aceito

## Contexto
Precisamos de uma solução de banco de dados para o projeto que seja simples de configurar, não exija instalação de servidores separados e seja adequada para um projeto educacional de curta duração.

## Decisão
Utilizaremos SQLite como banco de dados para o projeto, com Prisma como ORM para facilitar a interação.

## Justificativa
- **Banco de Dados Embutido**: SQLite é um banco de dados que funciona como um arquivo único, não requerendo um servidor separado.
- **Zero Configuração**: Não exige instalação ou configuração complexa, facilitando o início rápido do projeto.
- **Persistência Local**: Os dados são armazenados em um arquivo local, facilitando o backup e a portabilidade.
- **SQL Standard**: Permite aos alunos aprender e praticar SQL padrão, transferível para outros bancos de dados.
- **Prisma ORM**: Adiciona segurança de tipo e facilita operações CRUD com TypeScript.
- **Adequado para Desenvolvimento**: Perfeito para ambientes de desenvolvimento e aplicações com carga baixa.

## Consequências
- **Positivas**:
  - Setup extremamente rápido
  - Eliminação de dependências externas (servidores de banco de dados)
  - Facilidade para iniciantes entenderem os conceitos de persistência
  - Integração simples com Node.js

- **Negativas**:
  - Não é adequado para aplicações de produção com múltiplos usuários concorrentes
  - Recursos limitados em comparação com sistemas de banco de dados completos
  - Não ensina conceitos de conexão remota com bancos de dados

## Alternativas Consideradas
- **MongoDB**: Banco de dados NoSQL que ofereceria uma experiência diferente de modelagem de dados, mas requer instalação separada.
- **PostgreSQL/MySQL**: Sistemas de banco de dados relacionais completos, mais robustos, mas exigindo configuração de servidor.
- **Firebase/Firestore**: Solução serverless que eliminaria a necessidade de um backend tradicional, mas com curva de aprendizado diferente.
- **In-memory Database**: Ainda mais simples, mas sem persistência entre reinicializações da aplicação.
