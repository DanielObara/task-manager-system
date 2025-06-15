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

Após instalar o Prisma, certifique-se de executar:

```bash
npx prisma generate
```

Se encontrar erros como "@prisma/client did not initialize yet", consulte o documento `SOLUCAO_PROBLEMAS_PRISMA.md` para obter instruções detalhadas.

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
