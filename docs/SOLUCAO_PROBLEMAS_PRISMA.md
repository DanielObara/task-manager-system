# Solução de Problemas com Prisma

Este documento fornece soluções para problemas comuns encontrados ao trabalhar com o Prisma ORM no projeto Task Manager.

## Erro: "@prisma/client did not initialize yet"

Se você encontrar o erro:

```
Error: @prisma/client did not initialize yet. Please run "prisma generate" and try to import it again.
```

### Causa

Este erro ocorre quando o Prisma Client não consegue encontrar os arquivos necessários para se conectar ao banco de dados. Isso geralmente acontece por um dos seguintes motivos:

1. O comando `prisma generate` não foi executado após a instalação
2. O schema do Prisma foi modificado, mas o client não foi regenerado
3. Problemas com caminhos relativos para o arquivo schema.prisma

### Solução

Siga estes passos em ordem para resolver o problema:

#### 1. Instale as dependências do projeto

Primeiro, certifique-se de que todas as dependências foram instaladas corretamente:

```bash
# No diretório /task-manager/server
npm install
```

#### 2. Gere o Prisma Client

Execute o comando para gerar o Prisma Client:

```bash
# No diretório /task-manager/server
npx prisma generate
```

#### 3. Verifique o arquivo de schema

Certifique-se de que o arquivo `schema.prisma` existe no diretório `/task-manager/server/prisma/` e contém a definição correta do modelo de dados.

#### 4. Crie o banco de dados (se necessário)

Se o banco de dados ainda não existe, você precisa criar e aplicar as migrações:

```bash
# No diretório /task-manager/server
npx prisma migrate dev --name init
```

#### 5. Verifique a estrutura do projeto

A estrutura de diretórios deve ser semelhante a:

```
/task-manager
  /server
    /prisma
      /migrations
      schema.prisma
    /src
      /controllers
      /routes
      index.ts
    package.json
    tsconfig.json
```

#### 6. Reinicie o servidor

Após executar esses passos, reinicie o servidor:

```bash
# No diretório /task-manager/server
npm run dev
```

## Dicas Adicionais

### Windows e Caracteres Especiais nos Caminhos

Em sistemas Windows, caminhos com caracteres especiais (como acentos ou espaços) podem causar problemas. Tente:

1. Mover o projeto para um caminho sem caracteres especiais (ex: `C:\Projects\task-manager`)
2. Ou adicione uma configuração explícita para o caminho do schema no arquivo que inicializa o Prisma:

```typescript
// Em src/controllers/taskController.ts ou onde o PrismaClient é instanciado:
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient({
  datasources: {
    db: {
      url: `file:${__dirname}/../../prisma/dev.db`
    }
  }
});
```

### Configuração Explícita do Prisma

Se continuar enfrentando problemas, tente criar um arquivo separado para configuração do Prisma:

```typescript
// src/config/prisma.ts
import { PrismaClient } from '@prisma/client';
import path from 'path';

const prisma = new PrismaClient({
  datasources: {
    db: {
      url: `file:${path.resolve(__dirname, '../../prisma/dev.db')}`
    }
  }
});

export default prisma;
```

E então importe este arquivo em seus controladores em vez de instanciar o PrismaClient diretamente.
