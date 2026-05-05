---
layout: default
title: Guia do Administrador
nav_order: 3
permalink: /admin-guide
---

# ⚙️ Guia do Administrador

Estes controles existem na lateral esquerda dentro da tela de pesagem de quaisquer O.P. apenas caso seu usuário tenha privilégios adequados cadastrados no banco de dados.

---

## Visão Geral dos Privilégios

O sistema possui **três níveis** de acesso hierárquicos. Apenas o **Administrador** tem acesso completo a todas as funcionalidades. O quadro abaixo resume o que cada nível pode fazer:

| Funcionalidade | Administrador | Usuário | Operador |
|---|:---:|:---:|:---:|
| **Ver Ordens de Serviço (Dashboard)** | ✅ | ✅ | ✅ |
| **Iniciar/Continuar Pesagem** | ✅ | ✅ | ✅ |
| **Criar Nova OS** | ✅ | ✅ | ❌ |
| **Editar Quantidade de Bateladas** | ✅ | ❌ | ❌ |
| **Imprimir Códigos (OS + Bateladas)** | ✅ | ✅ | ✅ |
| **Gerenciar Ingredientes (CRUD)** | ✅ | ✅ | ❌ |
| **Gerenciar Receitas (CRUD)** | ✅ | ✅ | ❌ |
| **Gerenciar Usuários (CRUD)** | ✅ | ❌ | ❌ |
| **Ver Histórico de Pesagens** | ✅ | ✅ | ❌ |
| **Alterar Margem de Erro (%)** | ✅ | ❌ | ❌ |
| **Trocar Modo de Pesagem (Contínuo/Individual)** | ✅ | ❌ | ❌ |
| **Ativar Modo Manual Emergencial** | ✅ | ❌ | ❌ |
| **Bypass de Validação (Pular Scan QR)** | ✅ | ❌ | ❌ |
| **Acessar Menu de Navegação Admin** | ✅ | ✅ | ❌ |

> ⚠️ **Nota sobre o Operador**: O operador **só** visualiza o Dashboard de Ordens de Serviço e inicia a pesagem. Ele **não** tem acesso a nenhum menu administrativo, nem ao botão "Criar Nova OS".

---

## Menu de Navegação (Header)

O menu superior é adaptativo por nível de acesso. Os links visíveis são:

| Link | Rota | Administrador | Usuário | Operador |
|---|---|:---:|:---:|:---:|
| **OS** | `/admin/orders` | ✅ | ✅ | ❌ |
| **Usuários** | `/admin/users` | ✅ | ❌ | ❌ |
| **Ingredientes** | `/admin/ingredients` | ✅ | ✅ | ❌ |
| **Receitas** | `/admin/receitas` | ✅ | ✅ | ❌ |
| **Histórico** | `/admin/history` | ✅ | ✅ | ❌ |

Em dispositivos mobile, o menu colapsa em um **Sheet** lateral (botão hambúrguer) com os mesmos links filtrados por permissão.

---

## Funções Administrativas Principais

### 1. Status de Balança e Troubleshooting
Sempre observe a caixa de aviso verde / amarela / vermelha acerca do **Status da Balança Prix**.
- Caso conste como **Desconectada**, você pode clicar em **Abrir Janela de Conexão** e liberar o popup interativo do seu navegador web para confirmar a porta serial.
- Certifique-se de que nenhum software (como o "testador de conexão") está com a mesma porta `COM` aberta e travada em background.
- O status **Bloqueado** indica que o navegador não tem permissão para acessar hardware serial via HTTP — consulte o [Guia do Desenvolvedor](./developer-guide.md) para liberar a flag de segurança.

### 2. Configurações de Pesagem

As configurações ficam no **painel lateral esquerdo** durante a tela de pesagem (`/weighing/:id`). Apenas o administrador pode alterá-las; para outros usuários os controles ficam desabilitados (esmaecidos).

#### 2.1. Margem de Erro (%)
- Defina, por exemplo, como `1%`. Isso significa que se for para colocar 100 kg de Milho, de `99 kg` até `101 kg` a leitura constará como "Aceitável" e a O.P. prosseguirá.
- A margem se aplica tanto para cima (`+`) quanto para baixo (`-`) do peso alvo.
- Esse valor é salvo globalmente no `parametros.json` e persiste entre sessões.

#### 2.2. Modo Contínuo / Individual
- **Individual**: O peão tara a balança após terminar uma métrica, ou remove e usa um balde novo. A leitura da balança representa sempre o peso daquele ingrediente isolado.
- **Contínuo**: Tudo em uma grande betoneira; o sistema acumula o alvo sucessivamente. O peso alvo exibido é a soma acumulada de todos os ingredientes anteriores + o atual.

#### 2.3. Modo Manual Emergencial
- Se a balança estragar, ative esta opção. Você poderá digitar os valores coletados da pesagem de qualquer outra balança externa no sistema. 
- Os pesos inseridos devem estar sob sua vigilância para evitar desvios no controle de material.
- Quando ativo, a validação por scan de QR/código de barras também é desativada (bypass automático).
- O log registrará o modo como `'manual'` para auditoria.

---

## Funcionalidades Exclusivas do Administrador

### 3. Bypass de Validação de Ingrediente (Botão Escudo 🛡️)
Na tela de pesagem, ao lado do botão "Confirmar" do scan de ingrediente, existe um **botão com ícone de escudo** visível **apenas para administradores**. Ao clicar, ele pula a etapa de scan do QR/código de barras e marca o ingrediente como validado diretamente.

**Quando usar:** emergências onde o rótulo da sacaria está danificado, ilegível, ou o scanner não funciona.

### 4. Editar Quantidade de Bateladas de uma OS
No dashboard principal, cada OS "Pendente" ou "Em andamento" exibe um botão **"Editar Bateladas"** (ícone de camadas azul) apenas para administradores.

**Regras de negócio:**
- Não é possível definir a nova quantidade abaixo do número de bateladas **já concluídas**.
- Exemplo: se uma OS tem 5 bateladas e 3 já foram pesadas, o mínimo permitido é `3`.
- Se a nova quantidade for igual ao número de concluídas, a OS será automaticamente marcada como `concluida`.

### 5. Gerenciamento de Usuários (`/admin/users`)
Tela exclusiva para administradores, onde é possível:
- **Criar** novos usuários com nome, nível de acesso e código de acesso
- **Editar** dados de usuários existentes
- **Excluir** usuários
- Códigos de barras e QR são gerados automaticamente pelo sistema

### 6. Gerenciamento de Ordens de Serviço (`/admin/orders`)
Painel completo para:
- **Criar** novas OS vinculadas a receitas cadastradas
- **Definir quantidade** de bateladas necessárias
- **Marcar como urgente** (badge vermelho "URGENTE" no dashboard)
- **Editar** OS existentes
- **Excluir** OS

### 7. Gerenciamento de Ingredientes (`/admin/ingredients`)
Painel para:
- **Cadastrar** novos ingredientes com nome e estoque em kg
- **Editar** nome e estoque de ingredientes existentes
- **Excluir** ingredientes (⚠️ cuidado: receitas que referenciam o ingrediente podem quebrar)
- Visualizar códigos de barras e QR de cada ingrediente

### 8. Gerenciamento de Receitas (`/admin/receitas`)
Painel para:
- **Criar** receitas definindo nome, descrição e lista de ingredientes com pesos-alvo
- **Editar** receitas existentes (cada edição adiciona uma entrada no histórico de alterações)
- **Excluir** receitas
- **Visualizar** o histórico de alterações de cada receita

### 9. Histórico de Pesagens (`/admin/history`)
Permite visualizar todos os registros de pesagem (logs), contendo:
- Nome da receita pesada
- Data e hora da pesagem
- Operador que executou
- Modo de pesagem (manual ou automático)
- Snapshot completo dos ingredientes e pesos utilizados

---

## Impressão de Etiquetas

O sistema permite **imprimir etiquetas** com QR Code e código de barras para rastreabilidade. O botão "Imprimir Códigos" aparece em qualquer OS que tenha pelo menos uma batelada concluída.

### Tipos de Impressão

| Tipo | Conteúdo | Quando usar |
|---|---|---|
| **OS Geral** | QR + Barcode da Ordem de Serviço inteira | Etiquetar o lote/palete completo |
| **Batelada — Simples** | QR + Barcode da batelada (sem ingredientes) | Etiqueta rápida para cada saca/mistura |
| **Batelada — Auditoria** | QR + Barcode + tabela de ingredientes e pesos | Para controle de qualidade e rastreabilidade total |

### Formato da Etiqueta
- Tamanho: **80mm × 100mm** (compatível com impressoras térmicas de etiquetas)
- Conteúdo: título, subtítulo, data, QR Code centralizado, tabela de ingredientes (se auditoria), código de barras na base

---
> **Suporte Comercial e Técnico:** (51) 99231-8220 | (51) 99707-1562 | comercial@codars.com.br
