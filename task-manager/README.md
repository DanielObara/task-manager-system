# Instruções de Execução do Projeto

Este documento contém as instruções para executar o Sistema de Gerenciamento de Tarefas desenvolvido durante o desafio.

> **Nota Importante**: Durante a implementação, foram realizadas algumas adaptações ao código original para resolver problemas de contexto em callbacks. Consulte o arquivo `../docs/NOTA_IMPLEMENTACAO.md` para detalhes sobre essas alterações.

## Requisitos

- Node.js (versão 14+)
- npm ou yarn

## Estrutura do Projeto

O projeto está organizado em duas partes:
- `client`: Frontend React com TypeScript
- `server`: Backend Node.js com Express e TypeScript

## Passos para Execução

### 1. Preparando o Backend

1. Abra um terminal e navegue até a pasta do servidor:

```bash
cd task-manager/server
```

2. Instale as dependências:

```bash
npm install
```

3. Certifique-se de que o banco de dados SQLite está configurado corretamente (arquivo `.env` deve conter `DATABASE_URL="file:./dev.db"`).

4. Execute as migrações do Prisma para criar o banco de dados (se ainda não tiver feito):

```bash
npx prisma migrate dev --name init
```

5. Inicie o servidor:

```bash
npm run dev
```

O servidor será iniciado na porta 5000 (http://localhost:5000).

### 2. Preparando o Frontend

1. Abra outro terminal e navegue até a pasta do cliente:

```bash
cd task-manager/client
```

2. Instale as dependências:

```bash
npm install
```

3. Inicie a aplicação React:

```bash
npm start
```

A aplicação será aberta automaticamente em seu navegador padrão (http://localhost:3000).

## Testando o Aplicativo

1. Crie algumas tarefas usando o formulário "Nova Tarefa".
2. Experimente mudar o status das tarefas usando o seletor.
3. Teste o filtro de tarefas por status.
4. Exclua tarefas usando o botão "Excluir".
5. Verifique se as mudanças persistem mesmo após recarregar a página.

## Resolução de Problemas Comuns

### O backend não está conectando com o frontend

- Verifique se o servidor está rodando na porta 5000
- Confirme que a URL da API no arquivo `taskService.ts` está correta

### Erros de CORS

- Verifique se o middleware cors está configurado corretamente no servidor

### Erros do Prisma

- Execute `npx prisma generate` para atualizar o cliente Prisma
- Verifique se o banco de dados existe e está acessível

## Próximos Passos

Após completar o desafio básico, você pode experimentar:

1. Adicionar busca de tarefas por título
2. Implementar ordenação por data de criação
3. Adicionar datas de vencimento para as tarefas
4. Melhorar o design com bibliotecas de UI como Material-UI ou Chakra UI
