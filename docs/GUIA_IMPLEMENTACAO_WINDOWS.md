# Adendo ao Guia de Implementação: Instruções para Windows

Este documento fornece instruções específicas para usuários do Windows ao seguir o guia de implementação principal.

## Criação de Diretórios

No guia principal, quando vir comandos como este:

```bash
mkdir -p src/tests/{components,services}
```

No Windows, use o PowerShell com o seguinte comando equivalente:

```powershell
mkdir -Force src\__tests__\components, src\__tests__\services
```

Ou, se estiver usando o prompt de comando (CMD):

```cmd
mkdir "src\__tests__\components" "src\__tests__\services"
```

## Configuração do Prisma

Após instalar o Prisma, é fundamental executar:

```bash
npx prisma generate
```

No PowerShell, você pode adicionar isso como um script em seu package.json:

```json
"scripts": {
  "prisma:generate": "prisma generate",
  "prisma:migrate": "prisma migrate dev",
  "setup": "npm install && npm run prisma:generate"
}
```

E então executar:

```powershell
npm run prisma:generate
```

### Solução para erros comuns com Prisma no Windows

Se encontrar erros como "@prisma/client did not initialize yet", siga estas etapas:

1. **Gerar o Prisma Client**:
   ```powershell
   npm run prisma:generate
   ```

2. **Remover arquivos JavaScript duplicados que podem causar conflitos**:
   ```powershell
   Remove-Item .\src\controllers\*.js -ErrorAction SilentlyContinue
   Remove-Item .\src\routes\*.js -ErrorAction SilentlyContinue
   Remove-Item .\src\*.js -ErrorAction SilentlyContinue
   ```

3. **Corrigir o caminho de importação** (se necessário):
   Edite o arquivo `src\controllers\taskController.ts` e atualize a importação:
   ```typescript
   import { PrismaClient } from '../../generated/prisma';
   ```

4. **Verifique a estrutura de diretórios**:
   Após gerar o cliente Prisma, deve existir um diretório `/generated/prisma/` com os arquivos necessários.

## Caminhos de Arquivo

Ao seguir instruções que utilizam caminhos de arquivo:

- Substitua barras (`/`) por contrabarras (`\`) nos caminhos
- Evite usar caminhos com acentos ou caracteres especiais
- Se precisar acessar outro drive, use `cd /d D:\caminho` em vez de apenas `cd D:\caminho`

## Problemas com ESModules no Jest

Se encontrar problemas com o Jest e imports ESM (especialmente com Axios), adicione o seguinte ao seu `package.json` no cliente:

```json
"jest": {
  "transformIgnorePatterns": [
    "node_modules/(?!(axios)/)"
  ]
}
```

## Scripts npm

Em alguns casos, os scripts definidos no `package.json` podem precisar de pequenos ajustes para Windows. Por exemplo:

Ao invés de:
```json
"dev": "NODE_ENV=development nodemon src/index.ts"
```

Use:
```json
"dev": "cross-env NODE_ENV=development nodemon src/index.ts"
```

Para isso, instale o pacote `cross-env`:
```bash
npm install --save-dev cross-env
```

## Implementação de Controladores com Arrow Functions

É essencial implementar os métodos do controlador como arrow functions para evitar problemas com o contexto `this` no Windows:

```typescript
// Forma correta (use esta abordagem):
export class TaskController {
  getAllTasks = async (req: Request, res: Response) => {
    // Implementação
  };
}

// Forma que pode causar problemas (evite):
export class TaskController {
  async getAllTasks(req: Request, res: Response) {
    // Implementação
  }
}
```

Esta abordagem garante que o contexto `this` seja preservado quando os métodos são passados como callbacks para as rotas Express.
