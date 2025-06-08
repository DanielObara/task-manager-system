# ADR 1: Escolha do TypeScript como Linguagem de Programação

## Status
Aceito

## Contexto
Precisamos escolher uma linguagem de programação para o desenvolvimento do projeto que seja adequada para alunos iniciantes, mas que também introduza conceitos importantes para o desenvolvimento profissional.

## Decisão
Utilizaremos TypeScript tanto no frontend quanto no backend do projeto.

## Justificativa
- **Tipagem Estática**: TypeScript proporciona verificação de tipos em tempo de compilação, o que ajuda a evitar erros comuns e facilita a manutenção do código.
- **Superset de JavaScript**: Como TypeScript é um superset de JavaScript, os alunos podem utilizar seus conhecimentos prévios de JavaScript enquanto aprendem os benefícios da tipagem.
- **Adoção pela Indústria**: TypeScript tem sido amplamente adotado pela indústria, tornando-se uma habilidade valiosa para os alunos.
- **Ferramentas de Desenvolvimento**: A tipagem oferece melhor suporte em IDEs, com recursos de autocompletar e detecção de erros.
- **Escalabilidade**: Facilita a criação de código mais robusto e escalável, introduzindo conceitos de engenharia de software.

## Consequências
- **Positivas**:
  - Código mais seguro e previsível
  - Melhor experiência de desenvolvimento com suporte IDE
  - Transferência de conhecimento valiosa para o mercado de trabalho
  - Documentação implícita através de tipos

- **Negativas**:
  - Curva de aprendizado adicional para alunos que só conhecem JavaScript
  - Configuração inicial mais complexa
  - Tempo adicional para definição de interfaces e tipos

## Alternativas Consideradas
- **JavaScript puro**: Mais simples para iniciantes, mas sem os benefícios da tipagem estática
- **Java/C#**: Linguagens fortemente tipadas, mas com uma curva de aprendizado maior e ecossistema diferente para desenvolvimento web
