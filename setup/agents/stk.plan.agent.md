---
description: Executar o fluxo de trabalho de planejamento usando o respectivo template para gerar artefatos de design.
handoffs: 
  - label: Criar Tarefas
    agent: stk.tasks
    prompt: Divida o plano em tarefas
    send: true
model: GPT-5.2 (copilot)
---

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Estrutura

1. **Carregar contexto**: 
   - Ler `specs/[feature-name]/spec.md`
   - Caso não esteja no contexto, carregar a constituição #checkout-constitution.

2. **Executar o fluxo de planejamento**: Seguir a estrutura do template em markdown abaixo para:
   - Preencher o Contexto Técnico (marcar desconhecidos como "NEEDS CLARIFICATION")
   - Preencher a seção de Verificação da Constituição a partir da constituição
   - Avaliar os gates (ERRO se violações não justificadas)
   - Fase 0: Gerar research.md (resolver todos os "NEEDS CLARIFICATION")
   - Fase 1: Gerar data-model.md, contracts/, quickstart.md
   - Fase 1: Atualizar o contexto do agente executando o script do agente
   - Reavaliar a Verificação da Constituição pós-design

````markdown
# Plano de Implementação: [FUNCIONALIDADE]

**Branch**: `[feature-name]` | **Data**: [DATA] | **Especificação**: [link]  
**Entrada**: Especificação da funcionalidade em `/specs/[feature-name]/spec.md`

## Resumo

[Extraído da especificação da funcionalidade: requisito principal + abordagem técnica a partir da pesquisa]

## Contexto Técnico

<!--
  AÇÃO NECESSÁRIA: Substitua o conteúdo desta seção pelos detalhes técnicos
  do projeto. A estrutura aqui é apresentada em caráter orientativo para guiar
  o processo de iteração.
-->

**Linguagem/Versão**: [ex.: Python 3.11, Swift 5.9, Rust 1.75 ou NEEDS CLARIFICATION]  
**Dependências Principais**: [ex.: FastAPI, UIKit, LLVM ou NEEDS CLARIFICATION]  
**Armazenamento**: [se aplicável, ex.: PostgreSQL, CoreData, arquivos ou N/A]  
**Testes**: [ex.: pytest, XCTest, cargo test ou NEEDS CLARIFICATION]  
**Plataforma Alvo**: [ex.: servidor Linux, iOS 15+, WASM ou NEEDS CLARIFICATION]  
**Tipo de Projeto**: [único/web/mobile — determina a estrutura do código-fonte]  
**Metas de Performance**: [específico do domínio, ex.: 1000 req/s, 10k linhas/s, 60 fps ou NEEDS CLARIFICATION]  
**Restrições**: [específico do domínio, ex.: p95 <200ms, <100MB de memória, funciona offline ou NEEDS CLARIFICATION]  
**Escala/Escopo**: [específico do domínio, ex.: 10k usuários, 1M LOC, 50 telas ou NEEDS CLARIFICATION]

## Verificação da Constituição

*PORTÃO: Deve passar antes da pesquisa da Fase 0. Reavaliar após o design da Fase 1.*

[Portões definidos com base no arquivo de constituição]

## Estrutura do Projeto

### Documentação (desta funcionalidade)

```text
specs/[###-funcionalidade]/
├── plan.md              # Este arquivo (saída do comando /stk.plan)
├── research.md          # Saída da Fase 0 (/stk.plan)
├── data-model.md        # Saída da Fase 1 (/stk.plan)
├── quickstart.md        # Saída da Fase 1 (/stk.plan)
├── contracts/           # Saída da Fase 1 (/stk.plan)
└── tasks.md             # Saída da Fase 2 (/stk.tasks — NÃO criado por /stk.plan)
```

### Código-fonte (raiz do repositório)
<!--
  AÇÃO NECESSÁRIA: Substitua a árvore de exemplo abaixo pelo layout concreto
  desta funcionalidade. Remova opções não utilizadas e expanda a estrutura
  escolhida com caminhos reais (ex.: apps/admin, packages/algo). O plano final
  não deve incluir rótulos de "Opção".
-->

```text
# [REMOVA SE NÃO FOR USADO] Opção 1: Projeto único (PADRÃO)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [REMOVA SE NÃO FOR USADO] Opção 2: Aplicação web (quando "frontend" + "backend" detectados)
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [REMOVA SE NÃO FOR USADO] Opção 3: Mobile + API (quando "iOS/Android" detectado)
api/
└── [mesma estrutura do backend acima]

ios/ ou android/
└── [estrutura específica da plataforma: módulos de funcionalidade, fluxos de UI, testes da plataforma]
```

**Decisão de Estrutura**: [Documente a estrutura selecionada e referencie os diretórios reais capturados acima]

## Acompanhamento de Complexidade

> **Preencha SOMENTE se a Verificação da Constituição tiver violações que precisem ser justificadas**

| Violação | Por que é necessário | Alternativa mais simples rejeitada porque |
|---------|-----------------------|-------------------------------------------|
| [ex.: 4º projeto] | [necessidade atual] | [por que 3 projetos são insuficientes] |
| [ex.: Padrão Repository] | [problema específico] | [por que acesso direto ao DB é insuficiente] |
````

## Saídas

- `specs/[feature-name]/plan.md`
- `specs/[feature-name]/research.md`
- `specs/[feature-name]/data-model.md`
- `specs/[feature-name]/quickstart.md`
- `specs/[feature-name]/contracts/` (quando APIs estiverem no escopo)

## Fluxo de trabalho

1. Preencha **Technical Context** com valores concretos.
2. Preencha os “gates” de **Constitution Check** a partir da constituição que fornece os princípios e as diretrizes para o desenvolvimento de software.
   - Se alguma regra não negociável for violada, pare e gere um erro, a menos que a constituição
     permita explicitamente uma exceção.
3. Fase 0: produza `research.md` para resolver todas as incógnitas.
4. Fase 1: produza os artefatos de design (`data-model.md`, `contracts/`, `quickstart.md`).
5. Revalide a conformidade com a constituição após o design.

## Fases

### Fase 0: Esboço & Pesquisa

1. **Extrair desconhecidos do Contexto Técnico** acima:
   - Para cada NEEDS CLARIFICATION → tarefa de pesquisa
   - Para cada dependência → tarefa de melhores práticas
   - Para cada integração → tarefa de padrões

2. **Gerar e despachar agentes de pesquisa**:

   ```text
   Para cada desconhecido no Contexto Técnico:
     Tarefa: "Pesquisar {desconhecido} para {contexto da feature}"
   Para cada escolha de tecnologia:
     Tarefa: "Encontrar melhores práticas para {tecnologia} em {domínio}"
   ```

3. **Consolidar achados** em `research.md` usando o formato:
   - Decisão: [o que foi escolhido]
   - Justificativa: [por que foi escolhido]
   - Alternativas consideradas: [o que mais foi avaliado]

**Saída**: research.md com todos os "NEEDS CLARIFICATION" resolvidos

### Fase 1: Design & Contratos

**Pré-requisitos:** `research.md` completo

1. **Extrair entidades da especificação da feature** → `data-model.md`:
   - Nome da entidade, campos, relacionamentos
   - Regras de validação a partir dos requisitos
   - Transições de estado, se aplicável

2. **Gerar contratos de API** a partir dos requisitos funcionais:
   - Para cada ação do usuário → endpoint
   - Usar padrões REST/GraphQL comuns
   - Gerar OpenAPI/schema GraphQL em `/contracts/`

**Saída**: data-model.md, /contracts/*, quickstart.md, arquivo específico do agente

## Regras principais

- Usar caminhos absolutos
- ERRO em falhas de gate ou esclarecimentos não resolvidos