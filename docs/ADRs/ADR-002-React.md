# ADR 2: Escolha do React como Framework Frontend

## Status
Aceito

## Contexto
Precisamos escolher um framework frontend que permita a criação de interfaces interativas e seja acessível para alunos iniciantes, enquanto proporciona uma experiência de aprendizado valiosa.

## Decisão
Utilizaremos React com TypeScript para o desenvolvimento do frontend do projeto.

## Justificativa
- **Componentes Reutilizáveis**: O modelo baseado em componentes do React facilita a criação de interfaces modulares e reutilizáveis.
- **Virtual DOM**: A abordagem do React com Virtual DOM otimiza as atualizações de UI, proporcionando melhor performance.
- **Ecossistema Rico**: Grande quantidade de bibliotecas, ferramentas e recursos de aprendizado disponíveis.
- **Demanda do Mercado**: Habilidades em React são altamente valorizadas no mercado de trabalho.
- **Integração com TypeScript**: React tem excelente suporte para TypeScript, permitindo tipar componentes, props e estados.
- **Ferramentas de Desenvolvimento**: React DevTools e outras ferramentas facilitam a depuração e o desenvolvimento.

## Consequências
- **Positivas**:
  - Desenvolvimento de UI mais organizado e manutenível
  - Familiaridade com conceitos modernos de frontend (componentes, estado, props)
  - Preparação para frameworks mais avançados baseados em React (Next.js, Remix)
  - Comunidade ativa para suporte

- **Negativas**:
  - Conceitos como JSX e gerenciamento de estado podem ser desafiadores para iniciantes
  - Necessidade de ferramentas adicionais para roteamento, gerenciamento de formulários, etc.

## Alternativas Consideradas
- **Vue.js**: Potencialmente mais simples para iniciantes, mas com menor demanda no mercado
- **Angular**: Framework mais completo, mas com uma curva de aprendizado significativamente maior
- **Vanilla JavaScript**: Mais simples, mas sem os benefícios de um framework moderno
- **Svelte**: Inovador e eficiente, mas com ecossistema menor e menos recursos de aprendizado
