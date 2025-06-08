# Documentação de Testes

Este documento descreve a estratégia de testes adotada para o aplicativo Task Manager, incluindo detalhes sobre os tipos de testes implementados, as ferramentas utilizadas e as práticas recomendadas para manutenção e expansão da suíte de testes.

> **Nota**: Para informações sobre problemas específicos de implementação e soluções potenciais relacionadas aos testes, consulte o arquivo [NOTA_TESTES.md](./NOTA_TESTES.md).

## Visão Geral

Os testes foram implementados para garantir a qualidade do código, reduzir bugs e facilitar a manutenção do aplicativo. A estratégia de testes inclui:

1. **Testes Unitários**: Testam componentes individuais do código em isolamento
2. **Testes de Integração**: Testam a interação entre diferentes componentes

## Ferramentas Utilizadas

### Backend (Node.js/Express)

- **Jest**: Framework de testes
- **ts-jest**: Permite executar testes em TypeScript
- **Supertest**: Para testar APIs HTTP

### Frontend (React)

- **Jest**: Framework de testes
- **React Testing Library**: Para testar componentes React
- **Jest DOM**: Fornece assertions para DOM

## Configuração Especial

### Backend

O backend utiliza o ts-jest para processar arquivos TypeScript:

```javascript
// jest.config.js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  testMatch: ['**/__tests__/**/*.test.ts'],
  transform: {
    '^.+\\.ts$': 'ts-jest',
  },
  collectCoverage: true,
  coverageDirectory: 'coverage',
  collectCoverageFrom: [
    'src/**/*.ts',
    '!src/**/*.d.ts',
  ],
};
```

### Frontend

O frontend utiliza o Create React App que já vem com o Jest configurado. Para lidar com a versão moderna do Axios (ESM), utilizamos um mock global no arquivo `setupTests.js`:

```javascript
// setupTests.js
jest.mock('axios', () => ({
  get: jest.fn(() => Promise.resolve({ data: {} })),
  post: jest.fn(() => Promise.resolve({ data: {} })),
  put: jest.fn(() => Promise.resolve({ data: {} })),
  delete: jest.fn(() => Promise.resolve({})),
  defaults: {
    baseURL: '',
    headers: {
      common: {}
    }
  }
}));
```

Esta abordagem evita problemas de compatibilidade entre o ESM (ECMAScript Modules) usado pelo Axios moderno e o CommonJS usado pelo Jest por padrão.

## Estrutura dos Testes

### Backend

```
server/
├── __tests__/
│   ├── controllers/
│   │   └── taskController.test.ts
│   └── mocks/
│       └── prismaMock.ts
```

### Frontend

```
client/src/
├── __tests__/
│   ├── components/
│   │   ├── TaskList.test.tsx
│   │   └── TaskItem.test.tsx
│   └── services/
│       └── taskService.test.ts
```

## Tipos de Testes Implementados

### Backend

1. **Testes do Controller**:
   - Verificam se os endpoints REST funcionam corretamente
   - Testam diferentes cenários (sucesso, erro, dados inválidos)
   - Usam mocks para isolar o controller da camada de dados

### Frontend

1. **Testes de Serviços**:
   - Verificam se as chamadas à API são realizadas corretamente
   - Testam manipulação de dados e tratamento de erros
   - Usam mocks para simular respostas da API

2. **Testes de Componentes**:
   - Verificam se os componentes renderizam corretamente
   - Testam interações do usuário (cliques, inputs)
   - Verificam atualizações de estado e renderização condicional

## Como Executar os Testes

### Backend

```bash
cd task-manager/server
npm test
```

Para executar com cobertura:
```bash
npm run test:coverage
```

### Frontend

```bash
cd task-manager/client
npm test
```

## Práticas Recomendadas

1. **Manter testes independentes**: Cada teste deve ser independente dos outros
2. **Usar mocks adequadamente**: Isolar o código sendo testado de suas dependências
3. **Testar comportamentos, não implementação**: Focar no que o código faz, não em como ele faz
4. **Manter alta cobertura de código**: Buscar cobrir a maior parte do código com testes
5. **Executar testes regularmente**: Incluir testes no pipeline de CI/CD

## Ampliando os Testes

### Testes adicionais recomendados para implementação futura:

1. **Testes End-to-End (E2E)**:
   - Usar ferramentas como Cypress ou Playwright
   - Testar fluxos completos de usuário

2. **Testes de Desempenho**:
   - Avaliar o tempo de resposta da API
   - Verificar comportamento sob carga

3. **Testes de Acessibilidade**:
   - Verificar conformidade com WCAG
   - Garantir que o aplicativo seja utilizável por todos

## Conclusão

Os testes são uma parte essencial do desenvolvimento de software de qualidade. Esta abordagem de testes foi projetada para fornecer confiança no código, facilitando a manutenção e evolução do aplicativo Task Manager. À medida que o aplicativo cresce, a suíte de testes deve ser expandida para cobrir novas funcionalidades e casos de uso.
