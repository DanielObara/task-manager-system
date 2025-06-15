# Desafio de Desenvolvimento: Sistema de Gerenciamento de Tarefas

Este repositório contém um desafio de desenvolvimento para alunos iniciantes, projetado para ser completado em aproximadamente 2 horas. O objetivo é criar um sistema de gerenciamento de tarefas (Todo List) com operações CRUD básicas.

> **Atualização**: Foi adicionada uma nota de implementação (`docs/NOTA_IMPLEMENTACAO.md`) que documenta modificações feitas ao código original para resolver problemas de contexto nos callbacks do Express. Essas alterações refletem práticas recomendadas para lidar com o escopo do `this` em métodos de classe passados como callbacks.
>
> **Atualização de Testes**: Foi adicionada uma nota de atualização dos testes (`docs/NOTA_TESTES_ATUALIZACAO.md`) que documenta as correções feitas para resolver problemas de tipagem e testes assíncronos. Todos os 17 testes estão agora passando com sucesso.
>
> **Solução de Problemas Prisma**: Foi adicionado um guia (`docs/SOLUCAO_PROBLEMAS_PRISMA.md`) para resolver problemas comuns com o Prisma ORM, incluindo o erro "@prisma/client did not initialize yet".
>
> **Melhorias Implementadas**: Foi adicionado um documento (`docs/MELHORIAS_IMPLEMENTADAS.md`) que resume todas as melhorias feitas no projeto, incluindo configuração centralizada do Prisma, melhor tratamento de promessas nas rotas Express, e configuração do Jest para ESModules.

## Visão Geral

O desafio consiste em desenvolver uma aplicação full-stack utilizando:
- **Frontend**: React com TypeScript
- **Backend**: Node.js/Express com TypeScript
- **Banco de Dados**: SQLite com Prisma ORM
- **Testes**: Jest e React Testing Library

## Estrutura do Repositório

- `/docs` - Documentação do projeto
  - `PRD.md` - Documento de Requisitos do Produto
  - `CONTEXTO.md` - Contexto educacional do desafio  - `GUIA_IMPLEMENTACAO.md` - Guia passo a passo para implementação
  - `NOTA_IMPLEMENTACAO.md` - Notas sobre implementação e alterações realizadas
  - `DOCUMENTACAO_TESTES.md` - Documentação sobre a estratégia de testes  - `NOTA_TESTES.md` - Nota sobre problemas e soluções relacionados aos testes
  - `NOTA_TESTES_ATUALIZACAO.md` - Atualização das correções feitas nos testes
  - `SOLUCAO_PROBLEMAS_PRISMA.md` - Guia para solucionar problemas comuns com o Prisma
  - `GUIA_IMPLEMENTACAO_WINDOWS.md` - Instruções específicas para Windows
  - `MELHORIAS_IMPLEMENTADAS.md` - Resumo das melhorias implementadas no projeto
  - `/ADRs` - Architecture Decision Records explicando as escolhas técnicas
- `/task-manager` - Código fonte do projeto (será criado durante o desafio)

## Requisitos do Sistema

- Node.js (versão 14+)
- npm ou yarn
- Editor de código (VS Code recomendado)

## Como Iniciar

1. Leia a documentação na pasta `/docs` para entender os requisitos e o contexto do desafio
2. Siga o guia de implementação em `GUIA_IMPLEMENTACAO.md` para criar o projeto passo a passo
3. Consulte os ADRs para entender as decisões arquiteturais e tecnológicas

## Objetivos de Aprendizado

- Implementação de operações CRUD completas
- Integração entre frontend e backend
- Uso de TypeScript para tipagem segura
- Persistência de dados em banco de dados
- Desenvolvimento de API RESTful
- Implementação de testes unitários e de componentes

## Critérios de Avaliação

- Funcionalidade completa conforme especificações
- Qualidade e organização do código
- Uso adequado de TypeScript
- Interface de usuário funcional e responsiva
- Tratamento de erros básico
- Cobertura de testes adequada

## Desafios Adicionais (opcional)

Para alunos que concluírem o desafio rapidamente, sugestões de extensões:
- Adicionar sistema de filtros para as tarefas
- Implementar pesquisa por título/descrição
- Adicionar data de vencimento às tarefas
- Implementar categorias/tags para as tarefas
- Melhorar a experiência do usuário com animações

## Suporte

Se você tiver dúvidas durante o desafio, consulte:
- A documentação oficial das tecnologias utilizadas
- Os arquivos de documentação na pasta `/docs`
- Seu instrutor para questões específicas

---

Desenvolvido para fins educacionais - 2025
