# ADR 3: Escolha do Node.js como Plataforma Backend

## Status
Aceito

## Contexto
Precisamos de uma plataforma backend que seja compatível com o uso de TypeScript, adequada para iniciantes e capaz de suportar um serviço de API RESTful para o aplicativo de gerenciamento de tarefas.

## Decisão
Utilizaremos Node.js com Express (versão 4.18.2) como plataforma backend para o projeto.

## Justificativa
- **JavaScript no Backend**: Permite aos alunos utilizar a mesma linguagem (TypeScript/JavaScript) tanto no frontend quanto no backend, reduzindo a carga cognitiva.
- **Ecossistema NPM**: Acesso a um vasto ecossistema de pacotes que podem acelerar o desenvolvimento.
- **Não-bloqueante e Assíncrono**: O modelo de I/O não-bloqueante do Node.js é eficiente para operações de API.
- **Express.js**: Framework minimalista que facilita a criação de APIs RESTful com pouca configuração.
- **Versão Estável do Express**: Optamos pela versão 4.18.2 do Express por sua estabilidade, ampla documentação e compatibilidade com bibliotecas existentes.
- **Suporte a TypeScript**: Boa integração com TypeScript através de ts-node e tipos definidos (@types).
- **Baixo Consumo de Recursos**: Ideal para desenvolvimento local e ambientes com recursos limitados.

## Consequências
- **Positivas**:
  - Redução da barreira entre frontend e backend
  - Facilidade para iniciantes entenderem todo o fluxo de dados
  - Instalação e configuração rápidas
  - Execução eficiente de APIs com baixa carga

- **Negativas**:
  - Gestão de código assíncrono pode ser desafiadora para iniciantes
  - Necessidade de configuração adicional para usar TypeScript
  - Menos estruturado que frameworks como NestJS ou Spring Boot

## Alternativas Consideradas
- **NestJS**: Framework mais estruturado para Node.js, mas com maior complexidade inicial
- **Python com Flask/Django**: Sintaxe potencialmente mais amigável, mas introduziria uma segunda linguagem
- **Java com Spring Boot**: Fortemente tipado nativamente, mas com setup mais complexo e curva de aprendizado maior
- **Deno**: Runtime mais moderno para JavaScript/TypeScript, mas com ecossistema menor e menos material de aprendizado
