---
description: Execute o plano de implementação processando e executando todas as tarefas definidas em tasks.md
---

## Entrada do usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Roteiro

1. **Verificar o status das checklists** (se `specs/[feature-name]/checklists/` existir):
    - Examinar todos os arquivos de checklist no diretório checklists/
    - Para cada checklist, contar:
       - Itens totais: todas as linhas que correspondem a `- [ ]` ou `- [X]` ou `- [x]`
       - Itens concluídos: linhas correspondentes a `- [X]` ou `- [x]`
       - Itens pendentes: linhas correspondentes a `- [ ]`
    - Criar uma tabela de status:

     ```text
       | Checklist | Total | Concluídos | Pendentes | Status |
     |-----------|-------|-----------|------------|--------|
     | ux.md     | 12    | 12        | 0          | ✓ PASS |
     | test.md   | 8     | 5         | 3          | ✗ FAIL |
     | security.md | 6   | 6         | 0          | ✓ PASS |
     ```

    - Calcular o status geral:
       - **PASS**: todas as checklists têm 0 itens pendentes
       - **FAIL**: uma ou mais checklists têm itens pendentes

    - **Se alguma checklist estiver incompleta**:
       - Exibir a tabela com as contagens de itens pendentes
       - **PARAR** e perguntar: "Algumas checklists estão incompletas. Deseja prosseguir com a implementação mesmo assim? (sim/não)"
       - Aguardar a resposta do usuário antes de continuar
       - Se o usuário disser "no" ou "não" ou "wait" ou "aguarde" ou "stop" ou "pare", interromper a execução
       - Se o usuário disser "yes" ou "sim" ou "proceed" ou "prosseguir" ou "continue" ou "continuar", prosseguir para o passo 3

    - **Se todas as checklists estiverem completas**:
       - Exibir a tabela mostrando que todas as checklists passaram
       - Prosseguir automaticamente para o passo 3

2. Carregar e analisar o contexto de implementação:
   - **REQUIRED**: Ler tasks.md para obter a lista completa de tarefas e o plano de execução
   - **REQUIRED**: Ler plan.md para stack técnica, arquitetura e estrutura de arquivos
   - **IF EXISTS**: Ler data-model.md para entidades e relacionamentos
   - **IF EXISTS**: Ler contracts/ para especificações de API e requisitos de testes
   - **IF EXISTS**: Ler research.md para decisões técnicas e restrições
   - **IF EXISTS**: Ler quickstart.md para cenários de integração

3. **Verificação de configuração do projeto**:
   - **REQUIRED**: Criar/verificar arquivos de ignore com base na configuração real do projeto:

   **Lógica de detecção e criação**:
   - Verificar se o comando a seguir tem sucesso para determinar se o repositório é um repo git (criar/verificar .gitignore se for):

     ```sh
     git rev-parse --git-dir 2>/dev/null
     ```

   - Verificar se Dockerfile* existe ou se Docker aparece em plan.md → criar/verificar .dockerignore
   - Verificar se .eslintrc* existe → criar/verificar .eslintignore
   - Verificar se eslint.config.* existe → garantir que as entradas `ignores` do config cubram os padrões obrigatórios
   - Verificar se .prettierrc* existe → criar/verificar .prettierignore
   - Verificar se .npmrc ou package.json existe → criar/verificar .npmignore (se houver publicação)
   - Verificar se arquivos terraform (*.tf) existem → criar/verificar .terraformignore
   - Verificar se .helmignore é necessário (há charts Helm) → criar/verificar .helmignore

   **Se o arquivo de ignore já existir**: verificar se contém padrões essenciais; anexar apenas os padrões críticos faltantes
   **Se o arquivo de ignore não existir**: criar com o conjunto completo de padrões para a tecnologia detectada

   **Padrões comuns por tecnologia** (da stack em plan.md):
   - **Node.js/JavaScript/TypeScript**: `node_modules/`, `dist/`, `build/`, `*.log`, `.env*`
   - **Python**: `__pycache__/`, `*.pyc`, `.venv/`, `venv/`, `dist/`, `*.egg-info/`
   - **Java**: `target/`, `*.class`, `*.jar`, `.gradle/`, `build/`
   - **C#/.NET**: `bin/`, `obj/`, `*.user`, `*.suo`, `packages/`
   - **Go**: `*.exe`, `*.test`, `vendor/`, `*.out`
   - **Ruby**: `.bundle/`, `log/`, `tmp/`, `*.gem`, `vendor/bundle/`
   - **PHP**: `vendor/`, `*.log`, `*.cache`, `*.env`
   - **Rust**: `target/`, `debug/`, `release/`, `*.rs.bk`, `*.rlib`, `*.prof*`, `.idea/`, `*.log`, `.env*`
   - **Kotlin**: `build/`, `out/`, `.gradle/`, `.idea/`, `*.class`, `*.jar`, `*.iml`, `*.log`, `.env*`
   - **C++**: `build/`, `bin/`, `obj/`, `out/`, `*.o`, `*.so`, `*.a`, `*.exe`, `*.dll`, `.idea/`, `*.log`, `.env*`
   - **C**: `build/`, `bin/`, `obj/`, `out/`, `*.o`, `*.a`, `*.so`, `*.exe`, `Makefile`, `config.log`, `.idea/`, `*.log`, `.env*`
   - **Swift**: `.build/`, `DerivedData/`, `*.swiftpm/`, `Packages/`
   - **R**: `.Rproj.user/`, `.Rhistory`, `.RData`, `.Ruserdata`, `*.Rproj`, `packrat/`, `renv/`
   - **Universal**: `.DS_Store`, `Thumbs.db`, `*.tmp`, `*.swp`, `.vscode/`, `.idea/`

   **Padrões específicos por ferramenta**:
   - **Docker**: `node_modules/`, `.git/`, `Dockerfile*`, `.dockerignore`, `*.log*`, `.env*`, `coverage/`
   - **ESLint**: `node_modules/`, `dist/`, `build/`, `coverage/`, `*.min.js`
   - **Prettier**: `node_modules/`, `dist/`, `build/`, `coverage/`, `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`
   - **Terraform**: `.terraform/`, `*.tfstate*`, `*.tfvars`, `.terraform.lock.hcl`
   - **Kubernetes/k8s**: `*.secret.yaml`, `secrets/`, `.kube/`, `kubeconfig*`, `*.key`, `*.crt`

4. Interpretar a estrutura de tasks.md e extrair:
   - **Fases de tarefas**: Setup, Tests, Core, Integration, Polish
   - **Dependências de tarefas**: regras de execução sequencial vs paralela
   - **Detalhes de tarefas**: ID, descrição, caminhos de arquivos, marcadores de paralelismo [P]
   - **Fluxo de execução**: ordem e requisitos de dependência

5. Executar a implementação seguindo o plano de tarefas:
   - **Execução por fase**: concluir cada fase antes de passar para a próxima
   - **Respeitar dependências**: executar tarefas sequenciais em ordem; tarefas paralelas [P] podem rodar juntas  
   - **Seguir abordagem TDD**: executar tarefas de teste antes de suas tarefas de implementação correspondentes
   - **Coordenação por arquivo**: tarefas que afetam os mesmos arquivos devem rodar sequencialmente
   - **Checkpoints de validação**: verificar a conclusão de cada fase antes de prosseguir

6. Regras de execução da implementação:
   - **Setup primeiro**: inicializar estrutura do projeto, dependências e configuração
   - **Testes antes do código**: se precisar escrever testes para contratos, entidades e cenários de integração
   - **Desenvolvimento core**: implementar modelos, serviços, comandos de CLI, endpoints
   - **Trabalho de integração**: conexões com banco de dados, middleware, logging, serviços externos
   - **Polimento e validação**: testes unitários, otimização de performance, documentação

7. Acompanhamento de progresso e tratamento de erros:
   - Reportar progresso após cada tarefa concluída
   - Interromper a execução se qualquer tarefa não paralela falhar
   - Para tarefas paralelas [P], continuar com as bem-sucedidas e reportar as que falharam
   - Fornecer mensagens de erro claras com contexto para depuração
   - Sugerir próximos passos se a implementação não puder prosseguir
   - **IMPORTANTE**: para tarefas concluídas, marque a tarefa como [X] no arquivo de tasks.

8. Validação de conclusão:
   - Verificar se todas as tarefas obrigatórias foram concluídas
   - Checar se as funcionalidades implementadas correspondem à especificação original
   - Validar que os testes passam e que a cobertura atende aos requisitos
   - Confirmar que a implementação segue o plano técnico
   - Reportar status final com um resumo do trabalho concluído

Nota: Este comando pressupõe que existe uma decomposição completa de tarefas em tasks.md. Se as tarefas estiverem incompletas ou ausentes, sugira executar `/stk.tasks` primeiro para regenerar a lista de tarefas.
