---
layout: default
title: Página Inicial
---

# 📚 Documentação do Sistema de Pesagem 1.3

Bem-vindo à documentação oficial do Sistema de Pesagem para a Protege Nutrição Animal. O sistema proporciona uma maior confiabilidade e integração offline para a sua equipe operacional utilizando as balanças Prix 9096H da Toledo.

## Versão Atual: **1.3**
- Framework: Next.js 15 (App Router) + TypeScript
- Banco de Dados: Arquivos JSON locais (offline-first)
- Hardware: Web Serial API → Prix 9096H (9600 baud, 8N1)
- Autenticação: Session Storage com 3 níveis de acesso

---

## Selecione seu Guia

Escolha o guia de acordo com seu nível de acesso e necessidade:

- 📦 **[Documento de Entrega Técnica](./entrega-tecnica)**: Documento formal completo contendo resumo executivo, requisitos atendidos, arquitetura, modelo de dados, pré-requisitos de infraestrutura, plano de testes, limitações conhecidas, changelog, procedimento de backup e termo de aceite.

- 🧑‍💻 **[Guia do Usuário (Operador)](./user-guide)**: Tudo sobre como iniciar bateladas, validar ingredientes e fazer o processo de pesagem correto. Inclui layout da tela, fluxo passo-a-passo e resolução de problemas comuns.

- ⚙️ **[Guia do Administrador](./admin-guide)**: Instruções de parametrização da balança, configurações de "Modo Contínuo" e tolerância de peso. Detalha todas as features exclusivas do admin: bypass de validação, edição de bateladas, CRUD de usuários/ingredientes/receitas/OS, impressão de etiquetas e tabela completa de privilégios por nível.

- 💻 **[Guia do Desenvolvedor](./developer-guide)**: Para equipe de TI e mantenedores do projeto. Contém a arquitetura completa em diagramas Mermaid, todos os tipos de dados TypeScript com campos e relações (classDiagram + ERD), estrutura de pastas, fluxo de `npm install` e deploy standalone, protocolo serial documentado byte a byte, states do hook `useScale`, tabela de Server Actions, e troubleshooting de rede local.

- 📋 **[Blueprint Original](./blueprint)**: Especificação original do projeto com core features e style guidelines.

---
> **Suporte Comercial e Técnico:** (51) 99231-8220 | (51) 99707-1562 | comercial@codars.com.br
