---
description: Gerar um tasks.md acionável, ordenado por dependências, para a funcionalidade com base nos artefatos de design disponíveis.
handoffs: 
  - label: Implementar Projeto
    agent: stk.implement
    prompt: Inicie a implementação em fases
    send: true
model: GPT-5.2 (copilot)
---

## Entrada do Usuário

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Roteiro

1. **Carregar documentos de design**: Ler de `specs/[feature-name]/ `:
   - **Obrigatórios**: plan.md (stack, bibliotecas, estrutura), spec.md (histórias de usuário com prioridades)
   - **Opcionais**: data-model.md (entidades), contracts/ (endpoints de API), research.md (decisões), quickstart.md (cenários de validação)
   - Observação: nem todos os projetos possuem todos os documentos. Gere as tarefas com base no que estiver disponível.

3. **Executar o fluxo de geração de tarefas**:
   - Carregar plan.md e extrair stack, bibliotecas e estrutura do projeto
   - Carregar spec.md e extrair histórias de usuário com suas prioridades (P1, P2, P3, etc.)
   - Se data-model.md existir: extrair entidades e mapear para histórias de usuário
   - Se contracts/ existir: mapear endpoints para histórias de usuário
   - Se research.md existir: extrair decisões para tarefas de setup
   - Gerar tarefas organizadas por história de usuário (veja Regras de Geração de Tarefas abaixo)
   - Gerar grafo de dependências mostrando a ordem de conclusão das histórias
   - Criar exemplos de execução em paralelo por história
   - Validar completude (cada história tem todas as tarefas necessárias e é testável de forma independente)

4. **Gerar tasks.md**: Usar template em markdown abaixo como estrutura e preencher com:
   - Nome correto da feature a partir de plan.md
   - Fase 1: tarefas de Setup (inicialização do projeto)
   - Fase 2: tarefas Fundacionais (pré-requisitos bloqueantes para todas as histórias)
   - Fase 3+: uma fase por história de usuário (na ordem de prioridade de spec.md)
   - Cada fase inclui: objetivo da história, critério de teste independente, testes (se solicitados), tarefas de implementação
   - Fase final: polimento e preocupações transversais
   - Todas as tarefas devem seguir o formato estrito de checklist (veja Regras de Geração de Tarefas abaixo)
   - Caminhos de arquivo claros para cada tarefa
   - Seção de dependências mostrando a ordem de conclusão das histórias
   - Exemplos de execução em paralelo por história
   - Seção de estratégia de implementação (MVP primeiro, entrega incremental)

````markdown
---
description: "Template de lista de tarefas para implementação de funcionalidade"
---

# Tasks: [feature-name]

**Entrada**: Documentos de design em `/specs/[feature-name]/`
**Pré-requisitos**: plan.md (obrigatório), spec.md (obrigatório para histórias), research.md, data-model.md, contracts/

**Testes**: Os exemplos abaixo incluem tarefas de teste. Testes são OPCIONAIS — inclua apenas se forem explicitamente solicitados na especificação da funcionalidade.

**Organização**: As tarefas são agrupadas por história de usuário para permitir implementação e validação independentes de cada história.

## Formato: `[ID] [P?] [Story] Descrição`

- **[P]**: Pode ser feita em paralelo (arquivos diferentes, sem dependências)
- **[Story]**: A qual história de usuário esta tarefa pertence (ex.: US1, US2, US3)
- Inclua caminhos de arquivo exatos nas descrições

## Convenções de Caminhos

- **Projeto único**: `src/`, `tests/` na raiz do repositório
- **App web**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` ou `android/src/`
- Os caminhos abaixo assumem projeto único — ajuste conforme a estrutura definida em plan.md

<!-- 
  ============================================================================
  IMPORTANTE: As tarefas abaixo são APENAS EXEMPLOS para fins ilustrativos.
  
  O comando /stk.tasks DEVE substituir estes exemplos por tarefas reais com base em:
  - Histórias de usuário de spec.md (com suas prioridades P1, P2, P3...)
  - Requisitos da funcionalidade em plan.md
  - Entidades de data-model.md
  - Endpoints de contracts/
  
  As tarefas DEVEM ser organizadas por história de usuário para que cada história possa:
  - Ser implementada de forma independente
  - Ser testada de forma independente
  - Ser entregue como um incremento de MVP
  
  NÃO mantenha estas tarefas de exemplo no arquivo tasks.md gerado.
  ============================================================================
-->

## Fase 1: Setup (Infraestrutura Compartilhada)

**Objetivo**: Inicialização do projeto e estrutura básica

- [ ] T001 Criar estrutura do projeto conforme o plano de implementação
- [ ] T002 Inicializar o projeto em [language] com dependências do [framework]
- [ ] T003 [P] Configurar ferramentas de lint e formatação

---

## Fase 2: Fundacional (Pré-requisitos Bloqueantes)

**Objetivo**: Infraestrutura base que DEVE estar completa antes que QUALQUER história de usuário possa ser implementada

**⚠️ CRÍTICO**: Nenhum trabalho de história de usuário pode começar até que esta fase esteja completa

Exemplos de tarefas fundacionais (ajuste conforme o seu projeto):

- [ ] T004 Configurar schema de banco de dados e framework de migrations
- [ ] T005 [P] Implementar framework de autenticação/autorização
- [ ] T006 [P] Configurar roteamento de API e estrutura de middleware
- [ ] T007 Criar modelos/entidades base dos quais todas as histórias dependem
- [ ] T008 Configurar infraestrutura de tratamento de erros e logging
- [ ] T009 Configurar gestão de configuração de ambiente

**Checkpoint**: Fundacional pronto — a implementação das histórias pode começar em paralelo

---

## Fase 3: História de Usuário 1 - [Title] (Prioridade: P1) 🎯 MVP

**Objetivo**: [Breve descrição do que esta história entrega]

**Teste Independente**: [Como verificar que esta história funciona sozinha]

### Testes para a História de Usuário 1 (OPCIONAL — apenas se testes forem solicitados) ⚠️

> **NOTA: Escreva estes testes PRIMEIRO e garanta que FALHAM antes da implementação**

- [ ] T010 [P] [US1] Teste de contrato para [endpoint] em tests/contract/test_[name].py
- [ ] T011 [P] [US1] Teste de integração para [jornada do usuário] em tests/integration/test_[name].py

### Implementação da História de Usuário 1

- [ ] T012 [P] [US1] Criar model de [Entity1] em src/models/[entity1].py
- [ ] T013 [P] [US1] Criar model de [Entity2] em src/models/[entity2].py
- [ ] T014 [US1] Implementar [Service] em src/services/[service].py (depende de T012, T013)
- [ ] T015 [US1] Implementar [endpoint/feature] em src/[location]/[file].py
- [ ] T016 [US1] Adicionar validação e tratamento de erros
- [ ] T017 [US1] Adicionar logging para as operações da história 1

**Checkpoint**: Neste ponto, a História de Usuário 1 deve estar totalmente funcional e testável de forma independente

---

## Fase 4: História de Usuário 2 - [Title] (Prioridade: P2)

**Objetivo**: [Breve descrição do que esta história entrega]

**Teste Independente**: [Como verificar que esta história funciona sozinha]

### Testes para a História de Usuário 2 (OPCIONAL — apenas se testes forem solicitados) ⚠️

- [ ] T018 [P] [US2] Teste de contrato para [endpoint] em tests/contract/test_[name].py
- [ ] T019 [P] [US2] Teste de integração para [jornada do usuário] em tests/integration/test_[name].py

### Implementação da História de Usuário 2

- [ ] T020 [P] [US2] Criar model de [Entity] em src/models/[entity].py
- [ ] T021 [US2] Implementar [Service] em src/services/[service].py
- [ ] T022 [US2] Implementar [endpoint/feature] em src/[location]/[file].py
- [ ] T023 [US2] Integrar com componentes da História de Usuário 1 (se necessário)

**Checkpoint**: Neste ponto, as Histórias 1 E 2 devem funcionar de forma independente

---

## Fase 5: História de Usuário 3 - [Title] (Prioridade: P3)

**Objetivo**: [Breve descrição do que esta história entrega]

**Teste Independente**: [Como verificar que esta história funciona sozinha]

### Testes para a História de Usuário 3 (OPCIONAL — apenas se testes forem solicitados) ⚠️

- [ ] T024 [P] [US3] Teste de contrato para [endpoint] em tests/contract/test_[name].py
- [ ] T025 [P] [US3] Teste de integração para [jornada do usuário] em tests/integration/test_[name].py

### Implementação da História de Usuário 3

- [ ] T026 [P] [US3] Criar model de [Entity] em src/models/[entity].py
- [ ] T027 [US3] Implementar [Service] em src/services/[service].py
- [ ] T028 [US3] Implementar [endpoint/feature] em src/[location]/[file].py

**Checkpoint**: Todas as histórias de usuário devem estar funcionais de forma independente

---

[Adicione mais fases de histórias de usuário conforme necessário, seguindo o mesmo padrão]

---

## Fase N: Polimento & Preocupações Transversais

**Objetivo**: Melhorias que afetam múltiplas histórias de usuário

- [ ] TXXX [P] Atualizações de documentação em docs/
- [ ] TXXX Limpeza de código e refatoração
- [ ] TXXX Otimização de performance em todas as histórias
- [ ] TXXX [P] Testes unitários adicionais (se solicitados) em tests/unit/
- [ ] TXXX Endurecimento de segurança
- [ ] TXXX Executar validação do quickstart.md

---

## Dependências & Ordem de Execução

### Dependências entre Fases

- **Setup (Fase 1)**: Sem dependências — pode começar imediatamente
- **Fundacional (Fase 2)**: Depende do Setup — BLOQUEIA todas as histórias
- **Histórias (Fase 3+)**: Todas dependem da conclusão da fase Fundacional
  - Histórias podem então seguir em paralelo (se houver time/capacidade)
  - Ou sequencialmente por prioridade (P1 → P2 → P3)
- **Polimento (Fase Final)**: Depende de todas as histórias desejadas estarem completas

### Dependências entre Histórias de Usuário

- **História 1 (P1)**: Pode começar após Fundacional (Fase 2) — sem dependências de outras histórias
- **História 2 (P2)**: Pode começar após Fundacional (Fase 2) — pode integrar com US1, mas deve ser testável de forma independente
- **História 3 (P3)**: Pode começar após Fundacional (Fase 2) — pode integrar com US1/US2, mas deve ser testável de forma independente

### Dentro de Cada História de Usuário

- Testes (se incluídos) DEVEM ser escritos e FALHAR antes da implementação
- Models antes de services
- Services antes de endpoints
- Implementação núcleo antes de integração
- História completa antes de avançar para a próxima prioridade

### Oportunidades de Paralelismo

- Todas as tarefas de Setup marcadas com [P] podem rodar em paralelo
- Todas as tarefas fundacionais marcadas com [P] podem rodar em paralelo (dentro da Fase 2)
- Após a fase Fundacional, todas as histórias podem começar em paralelo (se houver capacidade no time)
- Todos os testes de uma história marcados com [P] podem rodar em paralelo
- Models dentro de uma história marcados com [P] podem rodar em paralelo
- Histórias diferentes podem ser trabalhadas em paralelo por pessoas diferentes

---

## Exemplo de Paralelismo: História de Usuário 1

```bash
# Dispare todos os testes da História 1 juntos (se testes forem solicitados):
Task: "Teste de contrato para [endpoint] em tests/contract/test_[name].py"
Task: "Teste de integração para [jornada do usuário] em tests/integration/test_[name].py"

# Dispare todos os models da História 1 juntos:
Task: "Criar model de [Entity1] em src/models/[entity1].py"
Task: "Criar model de [Entity2] em src/models/[entity2].py"
```

---

## Estratégia de Implementação

### MVP Primeiro (somente História 1)

1. Concluir Fase 1: Setup
2. Concluir Fase 2: Fundacional (CRÍTICO — bloqueia todas as histórias)
3. Concluir Fase 3: História 1
4. **PARE E VALIDE**: Teste a História 1 de forma independente
5. Deploy/demo se estiver pronto

### Entrega Incremental

1. Concluir Setup + Fundacional → Fundacional pronto
2. Adicionar História 1 → Testar de forma independente → Deploy/Demo (MVP!)
3. Adicionar História 2 → Testar de forma independente → Deploy/Demo
4. Adicionar História 3 → Testar de forma independente → Deploy/Demo
5. Cada história adiciona valor sem quebrar as anteriores

### Estratégia de Time em Paralelo

Com múltiplas pessoas desenvolvedoras:

1. Time conclui Setup + Fundacional em conjunto
2. Após Fundacional:
  - Dev A: História 1
  - Dev B: História 2
  - Dev C: História 3
3. Histórias concluem e integram de forma independente

---

## Notas

- Tarefas [P] = arquivos diferentes, sem dependências
- Rótulo [Story] mapeia tarefa para história específica (rastreabilidade)
- Cada história deve ser concluível e testável de forma independente
- Verifique que os testes falham antes de implementar
- Faça commit após cada tarefa ou grupo lógico
- Pare em qualquer checkpoint para validar a história de forma independente
- Evite: tarefas vagas, conflitos no mesmo arquivo, dependências entre histórias que quebrem independência
````

5. **Relatar**: Informar o caminho do tasks.md gerado e um resumo:
   - Contagem total de tarefas
   - Contagem de tarefas por história
   - Oportunidades de paralelismo identificadas
   - Critérios de teste independente para cada história
   - Escopo de MVP sugerido (tipicamente apenas a História de Usuário 1)
   - Validação de formato: confirmar que TODAS as tarefas seguem o checklist (checkbox, ID, labels, caminhos de arquivo)

Context for task generation: $ARGUMENTS

O tasks.md deve ser executável imediatamente — cada tarefa precisa ser específica o suficiente para que um LLM consiga concluí-la sem contexto adicional.

## Regras de Geração de Tarefas

**CRÍTICO**: As tarefas DEVEM ser organizadas por história de usuário para permitir implementação e teste independentes.

**Testes são OPCIONAIS**: só gere tarefas de teste se forem explicitamente solicitadas na especificação da feature ou se o usuário pedir uma abordagem TDD.

### Formato de Checklist (OBRIGATÓRIO)

Toda tarefa DEVE seguir estritamente este formato:

```text
- [ ] [TaskID] [P?] [Story?] Description with file path
```

**Componentes do Formato**:

1. **Checkbox**: SEMPRE começar com `- [ ]` (checkbox em Markdown)
2. **ID da Tarefa**: número sequencial (T001, T002, T003...) na ordem de execução
3. **Marcador [P]**: incluir SOMENTE se a tarefa for paralelizável (arquivos diferentes, sem dependências em tarefas incompletas)
4. **Rótulo [Story]**: OBRIGATÓRIO apenas nas tarefas das fases de histórias de usuário
   - Formato: [US1], [US2], [US3], etc. (mapeia para histórias de usuário de spec.md)
   - Fase de Setup: SEM rótulo de story
   - Fase Fundacional: SEM rótulo de story
   - Fases de Histórias: DEVEM ter rótulo de story
   - Fase de Polimento: SEM rótulo de story
5. **Descrição**: ação clara com caminho exato do arquivo

**Exemplos**:

- ✅ CORRECT: `- [ ] T001 Create project structure per implementation plan`
- ✅ CORRECT: `- [ ] T005 [P] Implement authentication middleware in src/middleware/auth.py`
- ✅ CORRECT: `- [ ] T012 [P] [US1] Create User model in src/models/user.py`
- ✅ CORRECT: `- [ ] T014 [US1] Implement UserService in src/services/user_service.py`
- ❌ WRONG: `- [ ] Create User model` (missing ID and Story label)
- ❌ WRONG: `T001 [US1] Create model` (missing checkbox)
- ❌ WRONG: `- [ ] [US1] Create User model` (missing Task ID)
- ❌ WRONG: `- [ ] T001 [US1] Create model` (missing file path)

### Organização das Tarefas

1. **A partir das Histórias de Usuário (spec.md)** — ORGANIZAÇÃO PRINCIPAL:
    - Cada história (P1, P2, P3...) vira sua própria fase
    - Mapear todos os componentes relacionados para a história:
       - Models necessários para aquela história
       - Services necessários para aquela história
       - Endpoints/UI necessários para aquela história
       - Se testes forem solicitados: testes específicos daquela história
    - Marcar dependências entre histórias (a maioria deve ser independente)

2. **A partir dos Contratos**:
   - Mapear cada contrato/endpoint → para a história que ele atende
   - Se testes forem solicitados: cada contrato → uma tarefa [P] de teste de contrato antes da implementação na fase daquela história

3. **A partir do Modelo de Dados**:
   - Mapear cada entidade para as histórias que precisam dela
   - Se a entidade servir múltiplas histórias: colocar na história mais cedo ou na fase de Setup
   - Relacionamentos → tarefas de camada de serviço na fase apropriada

4. **A partir de Setup/Infraestrutura**:
   - Infra compartilhada → fase de Setup (Fase 1)
   - Tarefas fundacionais/bloqueantes → fase Fundacional (Fase 2)
   - Setup específico da história → dentro da fase daquela história

### Estrutura de Fases

- **Fase 1**: Setup (inicialização do projeto)
- **Fase 2**: Fundacional (pré-requisitos bloqueantes — DEVE concluir antes das histórias)
- **Fase 3+**: Histórias de Usuário em ordem de prioridade (P1, P2, P3...)
   - Dentro de cada história: Testes (se solicitados) → Models → Services → Endpoints → Integração
   - Cada fase deve ser um incremento completo e testável de forma independente
- **Fase Final**: Polimento e Preocupações Transversais
