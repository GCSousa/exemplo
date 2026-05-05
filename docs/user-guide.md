---
layout: default
title: Guia do Usuário
nav_order: 5
permalink: /user-guide
---

# 🧑‍💻 Guia do Usuário (Operador)

O **Sistema de Pesagem** foi criado para agilizar o processo produtivo de suplementação e ração, além de evitar desperdícios e falhas durante a confecção de receitas de microingredientes.

---

## 1. Fazendo Login
Para começar, na tela de login (`/login`):
1. **Leia o seu crachá** usando um leitor de código de barras ou digite seu usuário,
2. **Digite sua senha** (código de acesso numérico),
3. Clique em **Entrar**.

> 💡 **Dica**: O sistema aceita tanto o nome do usuário quanto o código de barras ou QR Code do crachá como identificação.

---

## 2. Tela Inicial — Dashboard de Ordens de Serviço

Após o login, o operador é levado à tela principal (`/`), onde vizualiza as **Ordens de Serviço (OS)**.

### O que o operador vê na tela:

| Seção | Localização | Conteúdo |
|---|---|---|
| **Pendentes e Em Andamento** | Coluna da esquerda | Lista de OS que ainda precisam ser pesadas |
| **Concluídas Hoje** | Coluna da direita | OS que já foram finalizadas no dia |

### Elementos de cada OS:

- **Badge "URGENTE"** (vermelho): indica prioridade — essas OS aparecem no topo da lista
- **Nome da Receita**: qual ração/suplemento será preparado
- **Progresso**: ex: `2/5` = 2 bateladas concluídas de 5 planejadas
- **Botão "Iniciar Pesagem"** ou **"Continuar Pesagem"**: leva para a tela de pesagem

> ⚠️ **Nota**: Operadores **não** possuem o botão "Criar Nova OS" nem acesso aos menus de administração. Apenas as OS que estão aguardando serão exibidas.

---

## 3. O Fluxo de Pesagem

Ao clicar em "Iniciar Pesagem" ou "Continuar Pesagem", o operador é levado para a rota `/weighing/:id`.

### Layout da Tela de Pesagem

```
┌──────────────────────────────────────────────────────────────┐
│  [Header - Logo + Navegação + Sair]                          │
├─────────────────┬────────────────────────────────────────────┤
│                 │                                            │
│  PAINEL LATERAL │  ÁREA PRINCIPAL                            │
│  (Desktop only) │                                            │
│                 │  [← Voltar]                                │
│  ┌───────────┐  │  Pesagem: Nome da Receita                  │
│  │ Progresso │  │                                            │
│  │ da Receita│  │  ┌────────────────────────────┐            │
│  │           │  │  │ 1. VALIDAR: Ingrediente X  │            │
│  │ ○ Milho   │  │  │                            │            │
│  │ ● Soja  ←─│  │  │ [Campo de Scan / QR Code]  │            │
│  │ ○ Sorgo   │  │  │ [Confirmar]                │            │
│  │ ○ Núcleo  │  │  └────────────────────────────┘            │
│  └───────────┘  │                                            │
│                 │  ┌────────────────────────────┐            │
│  ┌───────────┐  │  │ 2. PESAR: Ingrediente X    │            │
│  │Configura- │  │  │                            │            │
│  │ções       │  │  │  Alvo: 25.00 kg            │            │
│  │           │  │  │  Margem: ±0.25 kg           │            │
│  │ Balança:  │  │  │                            │            │
│  │ 🟢 Conectado│ │  │  ┌────────────────┐        │            │
│  │           │  │  │  │   12.45  kg    │        │            │
│  │ Margem: 1%│  │  │  │  (Leitura Atual)│        │            │
│  │ Modo: Ind.│  │  │  └────────────────┘        │            │
│  │ Manual: ❌│  │  │                            │            │
│  └───────────┘  │  │  [✓ Próximo / Finalizar]   │            │
│                 │  └────────────────────────────┘            │
└─────────────────┴────────────────────────────────────────────┘
```

### Passo a Passo do Fluxo

O sistema não deixa erros acontecerem! Cada ingrediente deve ser seguido em uma ordem estrita:

#### Etapa 1 — Validação do Ingrediente
1. O primeiro card exibe **"1. Validar: [Nome do Ingrediente]"**.
2. **Escanear o QR Code ou Código de Barras** do ingrediente (sacaria ou recipiente) no campo de entrada.
3. Clique em **Confirmar** (ou pressione Enter).
4. Se o código bater com o ingrediente esperado, uma mensagem verde **"Item Validado ✅"** aparece.
5. **Se o código estiver errado**: um alerta vermelho "Código incorreto" aparece e o campo é **limpo automaticamente** para nova tentativa.
6. O card 2 (Pesagem) permanece **bloqueado e esmaecido** até que a validação seja concluída.

> 🔒 **Segurança**: isto evita a mistura de ingredientes trocados. Você **nunca** poderá pesar um ingrediente diferente do esperado.

#### Etapa 2 — Pesagem na Balança
1. Após a validação, o card **"2. Pesar: [Nome do Ingrediente]"** é ativado.
2. Coloque a caixa plástica / balde na balança e despeje o ingrediente.
3. O sistema mostra em tempo real:
   - **Alvo**: peso que deve ser atingido (ex: `25.00 kg`)
   - **Margem de tolerância**: variação aceitável (ex: `±0.25 kg` para 1%)
   - **Leitura Atual**: peso que a balança está mostrando, atualizado em tempo real
4. **Quando o peso entra na margem aceitável:**
   - A borda do quadro de leitura fica **VERDE**
   - O valor do peso fica **VERDE**
   - Um **contador de 5 segundos** começa: "Avançando em 5s... 4s... 3s..."
   - Se o peso permanecer estável na faixa por 5 segundos, o sistema **avança automaticamente** para o próximo ingrediente
5. Você também pode clicar no botão **"Próximo"** manualmente para avançar sem esperar os 5 segundos.
6. Se o peso **sair da faixa** antes dos 5 segundos, o contador é **cancelado** automaticamente.

#### Etapa 3 — Próximo Ingrediente
1. Ao avançar, os campos de scan e peso são **limpos automaticamente**.
2. O ingrediente atual no painel lateral muda (o anterior ganha ícone de check ✅ verde com fundo esmaecido).
3. Repita as etapas 1 e 2 para cada ingrediente da receita.

#### Etapa 4 — Finalizar
1. No último ingrediente, o botão muda para **"Finalizar"** em vez de "Próximo".
2. Ao finalizar:
   - O estoque de todos os ingredientes é **deduzido automaticamente**
   - Uma **batelada** é registrada na OS
   - Um log de pesagem é gravado com snapshot completo
   - Um diálogo de **"Concluído! ✅"** aparece
3. Clique em **"Início"** para voltar ao dashboard e iniciar outra batelada ou outra OS.

---

## 4. Painel Lateral (Desktop)

No canto esquerdo (visível apenas em telas grandes, ≥ 1024px), o painel lateral mostra:

### 4.1. Progresso da Receita
Lista de todos os ingredientes da receita com indicador visual:
- **●** → Ingrediente atual (seta e fundo destacado)
- **✅** → Ingrediente já pesado (esmaecido, mostrando peso real vs alvo)
- **○** → Ingrediente pendente

O operador pode clicar em ingredientes **não pesados** para pular a ordem, mas isso não é recomendado.

### 4.2. Configurações (Somente Leitura para Operador)
O operador pode **visualizar** mas **não alterar** as seguintes configurações:
- **Status da Balança**: verde (conectada), vermelha (desconectada), ou amarela (bloqueada)
- **Modo de Pesagem**: Individual ou Contínuo (definido pelo admin)
- **Margem de Erro**: porcentagem de tolerância (definida pelo admin)
- **Manual Emergencial**: se ativo, permite digitar o peso manualmente (definido pelo admin)

O botão **"Abrir Janela de Conexão"** para a balança está disponível para todos os usuários, pois a conexão serial depende do navegador/computador local.

---

## 5. Modos de Pesagem (Para Entendimento do Operador)

| Modo | Como funciona | O que o operador faz |
|---|---|---|
| **Individual** | Cada ingrediente é pesado separadamente. A balança é zerada/tarada entre ingredientes. | Pese um ingrediente, tare, pese o próximo. |
| **Contínuo** | Os ingredientes são adicionados um sobre o outro na mesma betoneira. O alvo exibido é acumulativo. | Continue despejando ingredientes — o alvo sobe automaticamente a cada avanço. |

---

## 6. O que fazer em caso de problemas?

| Problema | Solução |
|---|---|
| Balança mostra "Desconectada" | Clique "Abrir Janela de Conexão" e selecione a porta COM correta |
| Scan do ingrediente dá erro | Verifique se está escaneando o ingrediente **correto** para a etapa atual |
| Peso não muda | Verifique se o cabo da balança está conectado e nenhum outro programa está usando a porta |
| Campo de scan não limpa após erro | O sistema limpa automaticamente — se travou, atualize a página (F5) |
| Balança não aparece no popup | Outro software pode estar usando a porta COM. Feche programas como o Testador de Conexão |
| Status "Bloqueado" na balança | O navegador precisa de uma flag especial. Informe ao administrador (consulte o Guia do Desenvolvedor) |

---
> **Suporte Comercial e Técnico:** (51) 99231-8220 | (51) 99707-1562 | comercial@codars.com.br
