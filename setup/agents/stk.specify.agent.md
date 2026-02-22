---
description: Criar ou atualizar a especificação de funcionalidade a partir de uma descrição em linguagem natural.
handoffs: 
  - label: Executar Planejamento Técnico
    agent: stk.plan
    prompt: Crie um plano para a especificação
    send: true
model: GPT-5.2 (copilot)
---

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Estrutura

O texto que o usuário digitou após `/stk.specify` na mensagem que acionou o comando **é** a descrição da funcionalidade. Assuma que você sempre a tem disponível nesta conversa mesmo que `$ARGUMENTS` apareça literalmente abaixo. Não peça ao usuário para repeti-la, a menos que ele tenha fornecido um comando vazio.

Dada essa descrição da funcionalidade, faça o seguinte:

1. **Gere um nome curto e conciso** (2–4 palavras) para o nome da feature (funcionalidade):
   - Analise a descrição e extraia as palavras‑chave mais significativas
   - Crie um nome curto (2–4 palavras) que capture a essência da funcionalidade
   - Use o formato ação‑substantivo quando possível (ex.: "add-user-auth", "fix-payment-bug")
   - Preserve termos técnicos e siglas (OAuth2, API, JWT etc.)
   - Mantenha conciso, mas descritivo o suficiente para entender a funcionalidade rapidamente
   - Exemplos:
     - "I want to add user authentication" → "user-auth"
     - "Implement OAuth2 integration for the API" → "oauth2-api-integration"
     - "Create a dashboard for analytics" → "analytics-dashboard"
     - "Fix payment processing timeout bug" → "fix-payment-timeout"

2. Use a estrutura de template em markdown abaixo para criar a especificação:
   - Objetivo: Gerar uma especificação de funcionalidade (`spec.md`) que capture cenários de uso, requisitos e critérios de sucesso.
   - Entradas: 
     - Descrição do usuário / problem statement.
     - Constituição: Carregar a constituição que fornece os princípios e as diretrizes para o desenvolvimento de software.
   - Saída: `specs/[feature-name]/spec.md`

```markdown
# Especificação de Funcionalidade: [NOME DA FUNCIONALIDADE]

**Funcionalidade**: `[feature-name]`  
**Criado em**: [DATA]  
**Status**: Rascunho  
**Entrada**: Descrição do usuário: "$ARGUMENTS"

## Cenários de Usuário & Testes *(obrigatório)*

<!--
  IMPORTANTE: As histórias de usuário devem ser PRIORIZADAS como jornadas de usuário, ordenadas por importância.
  Cada história/jornada deve ser TESTÁVEL DE FORMA INDEPENDENTE — ou seja, se você implementar apenas UMA delas,
  ainda deve existir um MVP (Produto Mínimo Viável) que entregue valor.
  
  Atribua prioridades (P1, P2, P3, etc.) a cada história, onde P1 é a mais crítica.
  Pense em cada história como uma fatia independente de funcionalidade que pode ser:
  - Desenvolvida de forma independente
  - Testada de forma independente
  - Implantada de forma independente
  - Demonstrada aos usuários de forma independente
-->

### História de Usuário 1 - [Título Breve] (Prioridade: P1)

[Descreva esta jornada do usuário em linguagem simples]

**Por que esta prioridade**: [Explique o valor e por que isso tem este nível de prioridade]

**Teste Independente**: [Descreva como isso pode ser testado de forma independente — ex.: "Pode ser totalmente testado por meio de [ação específica] e entrega [valor específico]"]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]
2. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

---

### História de Usuário 2 - [Título Breve] (Prioridade: P2)

[Descreva esta jornada do usuário em linguagem simples]

**Por que esta prioridade**: [Explique o valor e por que isso tem este nível de prioridade]

**Teste Independente**: [Descreva como isso pode ser testado de forma independente]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

---

### História de Usuário 3 - [Título Breve] (Prioridade: P3)

[Descreva esta jornada do usuário em linguagem simples]

**Por que esta prioridade**: [Explique o valor e por que isso tem este nível de prioridade]

**Teste Independente**: [Descreva como isso pode ser testado de forma independente]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

---

[Adicione mais histórias de usuário conforme necessário, cada uma com uma prioridade atribuída]

### Casos Limite

<!--
  AÇÃO NECESSÁRIA: O conteúdo desta seção representa placeholders.
  Preencha com os casos limite corretos.
-->

- O que acontece quando [condição de limite]?
- Como o sistema lida com [cenário de erro]?

## Requisitos *(obrigatório)*

<!--
  AÇÃO NECESSÁRIA: O conteúdo desta seção representa placeholders.
  Preencha com os requisitos funcionais corretos.
-->

### Requisitos Funcionais

- **FR-001**: O sistema DEVE [capacidade específica, ex.: "permitir que usuários criem contas"]
- **FR-002**: O sistema DEVE [capacidade específica, ex.: "validar endereços de e-mail"]  
- **FR-003**: Usuários DEVEM conseguir [interação-chave, ex.: "redefinir sua senha"]
- **FR-004**: O sistema DEVE [requisito de dados, ex.: "persistir preferências do usuário"]
- **FR-005**: O sistema DEVE [comportamento, ex.: "registrar todos os eventos de segurança"]

### Capacidades do Serviço (para Conformidade com a Constituição)

Declare quais capacidades este recurso/serviço utilizará. Essas seleções acionam requisitos condicionais não negociáveis da **Constituição**.

- APIs REST: [Sim/Não]
- Persistência (banco de dados): [Sim/Não]
- Integrações AWS (escolha quaisquer que se apliquem): [Nenhuma / S3 / SNS / SQS / Secrets Manager / Outra]

*Exemplo de como marcar requisitos indefinidos:*

- **FR-006**: O sistema DEVE autenticar usuários via [NEEDS CLARIFICATION: método de autenticação não especificado — e-mail/senha, SSO, OAuth?]
- **FR-007**: O sistema DEVE reter dados do usuário por [NEEDS CLARIFICATION: período de retenção não especificado]

### Entidades-Chave *(inclua se a funcionalidade envolver dados)*

- **[Entidade 1]**: [O que representa; atributos principais sem detalhes de implementação]
- **[Entidade 2]**: [O que representa; relacionamentos com outras entidades]

## Critérios de Sucesso *(obrigatório)*

<!--
  AÇÃO NECESSÁRIA: Defina critérios de sucesso mensuráveis.
  Eles devem ser agnósticos de tecnologia e mensuráveis.
-->

### Resultados Mensuráveis

- **SC-001**: [Métrica mensurável, ex.: "Usuários conseguem concluir a criação de conta em menos de 2 minutos"]
- **SC-002**: [Métrica mensurável, ex.: "O sistema suporta 1000 usuários simultâneos sem degradação"]
- **SC-003**: [Métrica de satisfação do usuário, ex.: "90% dos usuários concluem a tarefa principal na primeira tentativa"]
- **SC-004**: [Métrica de negócio, ex.: "Reduzir chamados de suporte relacionados a [X] em 50%"]
```

3. Siga este fluxo de execução:

   1. Interpretar a descrição do usuário a partir da Entrada  
      Se estiver vazia: ERRO "No feature description provided" (Nenhuma descrição de funcionalidade fornecida)
   2. Extrair conceitos‑chave da descrição  
      Identifique: atores, ações, dados, restrições
   3. Para aspectos pouco claros:
      - Faça suposições informadas com base no contexto e em padrões do setor
      - Só marque com [NEEDS CLARIFICATION: pergunta específica] se:
        - A escolha impacta significativamente o escopo da funcionalidade ou a experiência do usuário
        - Existem múltiplas interpretações razoáveis com implicações diferentes
        - Não existe um padrão/default razoável
      - **LIMITE: no máximo 3 marcadores [NEEDS CLARIFICATION] no total**
      - Priorize esclarecimentos por impacto: escopo > segurança/privacidade > experiência do usuário > detalhes técnicos
   4. Preencher a seção Cenários de Usuário & Testes  
      Se não houver um fluxo de usuário claro: ERRO "Cannot determine user scenarios" (Não foi possível determinar cenários de usuário)
   5. Gerar Requisitos Funcionais  
      Cada requisito deve ser testável  
      Use padrões/defaults razoáveis para o que não foi especificado (documente as suposições na seção Assumptions)
   6. Definir Critérios de Sucesso  
      Crie resultados mensuráveis e agnósticos de tecnologia  
      Inclua métricas quantitativas (tempo, performance, volume) e qualitativas (satisfação do usuário, conclusão de tarefa)  
      Cada critério deve ser verificável sem detalhes de implementação
   7. Identificar Entidades‑Chave (se houver dados envolvidos)
   8. Retornar: SUCCESS (especificação pronta para planejamento)

4. Escreva a especificação em `specs/[feature-name]/spec.md` usando a estrutura do template, substituindo placeholders por detalhes concretos derivados da descrição da funcionalidade (argumentos), preservando a ordem e os títulos das seções.

5. **Validação de Qualidade da Especificação**: Após escrever a especificação inicial, valide-a com base em critérios de qualidade:

   a. **Criar Checklist de Qualidade da Especificação**: Gere um arquivo de checklist em `specs/[feature-name]/checklists/requirements.md` usando a estrutura do template de checklist com estes itens de validação:

   ```markdown
   # Checklist de Qualidade da Especificação: [NOME DA FUNCIONALIDADE]

   **Objetivo**: Validar a completude e a qualidade da especificação antes de prosseguir para o planejamento  
   **Criado em**: [DATA]  
   **Funcionalidade**: [Link para spec.md]

   ## Qualidade do Conteúdo

   - [ ] Sem detalhes de implementação (linguagens, frameworks, APIs)
   - [ ] Focado no valor para o usuário e nas necessidades do negócio
   - [ ] Escrito para stakeholders não técnicos
   - [ ] Todas as seções obrigatórias foram preenchidas

   ## Completude dos Requisitos

   - [ ] Não restam marcadores [NEEDS CLARIFICATION]
   - [ ] Os requisitos são testáveis e não ambíguos
   - [ ] Os critérios de sucesso são mensuráveis
   - [ ] Os critérios de sucesso são agnósticos de tecnologia (sem detalhes de implementação)
   - [ ] Todos os cenários de aceitação estão definidos
   - [ ] Casos limite foram identificados
   - [ ] O escopo está claramente delimitado
   - [ ] Dependências e suposições foram identificadas

   ## Prontidão da Funcionalidade

   - [ ] Todos os requisitos funcionais têm critérios de aceitação claros
   - [ ] Os cenários de usuário cobrem os fluxos principais
   - [ ] A funcionalidade atende aos resultados mensuráveis definidos nos Critérios de Sucesso
   - [ ] Nenhum detalhe de implementação vazou para a especificação

   ## Notas

   - Itens marcados como incompletos exigem atualizações na especificação antes de prosseguir para `/stk.plan`
   ```

   b. **Executar a Checagem de Validação**: Revise a especificação contra cada item do checklist:
      - Para cada item, determine se passa ou falha
      - Documente problemas específicos encontrados (citando trechos relevantes da especificação)

   c. **Tratar Resultados da Validação**:

      - **Se todos os itens passarem**: marque o checklist como completo e prossiga para o passo 6

      - **Se itens falharem (excluindo [NEEDS CLARIFICATION])**:
        1. Liste os itens que falharam e os problemas específicos
        2. Atualize a especificação para resolver cada problema
        3. Reexecute a validação até que tudo passe (máx. 3 iterações)
        4. Se ainda falhar após 3 iterações, documente os problemas restantes nas notas do checklist e avise o usuário

      - **Se ainda houver marcadores [NEEDS CLARIFICATION]**:
        1. Extraia todos os marcadores [NEEDS CLARIFICATION: ...] da especificação
        2. **CHECAGEM DO LIMITE**: se houver mais de 3 marcadores, mantenha apenas os 3 mais críticos (por impacto em escopo/segurança/UX) e faça suposições informadas para o restante
        3. Para cada esclarecimento necessário (máx. 3), apresente opções ao usuário neste formato:

         ```markdown
         ## Pergunta [N]: [Tópico]

         **Contexto**: [Cite a seção relevante da especificação]

         **O que precisamos saber**: [Pergunta específica do marcador NEEDS CLARIFICATION]

         **Respostas sugeridas**:

         | Opção         | Resposta                     | Implicações                                        |
         |---------------|------------------------------|----------------------------------------------------|
         | A             | [Primeira resposta sugerida] | [O que isso significa para a funcionalidade]       |
         | B             | [Segunda resposta sugerida]  | [O que isso significa para a funcionalidade]       |
         | C             | [Terceira resposta sugerida] | [O que isso significa para a funcionalidade]       |
         | Personalizado | Forneça sua própria resposta | [Explique como fornecer uma entrada personalizada] |

         **Sua escolha**: _[Aguardar resposta do usuário]_
         ```

        4. **CRÍTICO — Formatação de Tabelas**: garanta que tabelas Markdown estejam corretamente formatadas:
           - Use espaçamento consistente e pipes alinhados
           - Cada célula deve ter espaços ao redor do conteúdo: `| Conteúdo |` e não `|Conteúdo|`
           - O separador do cabeçalho deve ter pelo menos 3 hifens: `|--------|`
           - Verifique se a tabela renderiza corretamente no preview do Markdown
        5. Numere as perguntas sequencialmente (Q1, Q2, Q3 — máximo 3 no total)
        6. Apresente todas as perguntas juntas antes de aguardar respostas
        7. Aguarde o usuário responder com suas escolhas para todas as perguntas (ex.: "Q1: A, Q2: Custom - [detalhes], Q3: B")
        8. Atualize a especificação substituindo cada marcador [NEEDS CLARIFICATION] pela resposta selecionada/fornecida pelo usuário
        9. Reexecute a validação após todos os esclarecimentos serem resolvidos

   d. **Atualizar o Checklist**: após cada iteração de validação, atualize o arquivo do checklist com o status atual de aprovação/reprovação

7. Reporte a conclusão com nome da funcionalidade, caminho do arquivo da especificação, resultados do checklist e prontidão para a próxima fase (`/stk.plan`).

**OBS.:** O script cria e faz checkout da nova branch e inicializa o arquivo da especificação antes de escrever.

## Diretrizes Gerais

## Diretrizes Rápidas

- Foque no **O QUÊ** os usuários precisam e **POR QUÊ**.
- Evite o COMO implementar (sem stack, APIs, estrutura de código).
- Escreva para stakeholders de negócio, não para desenvolvedores.
- NÃO crie checklists embutidos na especificação. Isso será um comando separado.

### Requisitos de Seção

- **Seções obrigatórias**: devem ser preenchidas para toda funcionalidade
- **Seções opcionais**: inclua apenas quando forem relevantes
- Quando uma seção não se aplicar, remova-a completamente (não deixe como "N/A")

### Para Geração por IA

Ao criar esta especificação a partir de um prompt do usuário:

1. **Faça suposições informadas**: use contexto, padrões do setor e padrões comuns para preencher lacunas
2. **Documente suposições**: registre padrões/defaults razoáveis na seção Assumptions
3. **Limite esclarecimentos**: no máximo 3 marcadores [NEEDS CLARIFICATION] — use apenas para decisões críticas que:
   - Impactem significativamente o escopo ou a experiência do usuário
   - Tenham múltiplas interpretações razoáveis com implicações distintas
   - Não tenham um default razoável
4. **Priorize esclarecimentos**: escopo > segurança/privacidade > UX > detalhes técnicos
5. **Pense como testador**: todo requisito vago deve falhar no item "testável e não ambíguo"
6. **Áreas comuns que precisam de esclarecimento** (somente se não houver default razoável):
   - Escopo e limites da funcionalidade (incluir/excluir casos de uso)
   - Tipos de usuário e permissões (se houver interpretações conflitantes)
   - Requisitos de segurança/conformidade (quando legal/financeiramente significativo)

**Exemplos de defaults razoáveis** (não pergunte sobre isso):

- Retenção de dados: práticas padrão do setor para o domínio
- Metas de performance: expectativas padrão de web/mobile se não especificado
- Tratamento de erros: mensagens amigáveis com fallbacks apropriados
- Método de autenticação: sessão padrão ou OAuth2 para apps web
- Padrões de integração: APIs RESTful se não especificado

### Diretrizes de Critérios de Sucesso

Critérios de sucesso devem ser:

1. **Mensuráveis**: incluir métricas específicas (tempo, %, contagem, taxa)
2. **Agnósticos de tecnologia**: sem mencionar frameworks, linguagens, bancos, ferramentas
3. **Focados no usuário**: resultados do ponto de vista do usuário/negócio, não internals do sistema
4. **Verificáveis**: podem ser testados/validados sem conhecer detalhes de implementação

**Bons exemplos**:

- "Usuários conseguem concluir o checkout em menos de 3 minutos"
- "O sistema suporta 10.000 usuários concorrentes"
- "95% das buscas retornam resultados em menos de 1 segundo"
- "A taxa de conclusão da tarefa melhora em 40%"

**Maus exemplos** (focados em implementação):

- "Tempo de resposta da API abaixo de 200ms" (muito técnico; use "Usuários veem resultados rapidamente")
- "O banco de dados suporta 1000 TPS" (detalhe de implementação; use uma métrica orientada ao usuário)
- "Componentes React renderizam eficientemente" (específico de framework)
- "Cache hit rate do Redis acima de 80%" (tecnologia específica)