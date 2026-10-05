# Changelog

Todas as mudanças notáveis deste projeto são documentadas neste arquivo, no formato
[Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/), com
[versionamento semântico](https://semver.org/lang/pt-BR/): mudanças no fluxo da skill são
versões *major*, exemplos e documentação são *minor*, correções são *patch*.

## [3.0.0] — 2026-10-05 — Requisitos rastreáveis e aprovação registrada

Major pela regra deste repositório: a mudança altera critérios de saída de fases do fluxo.

### Adicionado

- **Requisitos rastreáveis (Fase 1)**: linhas `**R<n>** …`, cada uma com ao menos um critério
  de aceite em GWT ou EARS, e leitura do PRD relacionado em `docs/prd/` quando houver.
- **Marcador `[ESCLARECER: …]`** para dúvidas sem resposta e seção `## Esclarecimentos` para o
  registro de pergunta e resposta.
- **Aprovação gravada (Fase 3)**: `Status: aprovada`, `Aprovado por` e `Aprovado em` no
  cabeçalho da spec, em commit próprio, e spec apresentada só com zero marcadores abertos.
- **Rastreio por requisito**: tarefas do plano terminam com `(R<n>)` (Fase 4), o teste vermelho
  cita o R<n> (Fase 6) e a revisão adversarial registra a tabela `| R | veredito | evidência |`
  (Fase 7).
- Erro comum "aprovar só na conversa".

### Modificado

- **Fase 8**: a spec passa a `Status: arquivada` no encerramento.
- Critérios de saída das Fases 1, 3 e 7 atualizados.
- README, diagrama Mermaid e exemplo de Tier 2 coerentes com o novo fluxo.

## [2.0.0] — 2026-07-08 — Rigor proporcional por tiers (SDD 2.0)

### Adicionado

- **Fase 0 — Contexto e classificação**: leitura obrigatória da constituição (`AGENTS.md` ou
  similar), do steering roteado e da memória do projeto (`docs/PROJECT_MEMORY.md`) antes de
  qualquer ação; classificação da mudança em Tier 0/1/2 com critérios objetivos, regra "na
  dúvida, tier mais alto" e escalação obrigatória.
- **Matriz tier → fases**: Tier 0 vai direto ao commit; Tier 1 percorre plano leve → TDD →
  verificação; Tier 2 percorre o ciclo completo.
- **Detecção scaffold-aware**: a skill reconhece e usa os artefatos de governança do
  `sdd-scaffold` quando presentes, com degradação graciosa para repositórios comuns.
- **Gate arquitetural ampliado**: além de breaking changes, a Fase 2 cobre o checklist de
  Impacto Arquitetural da Arquitetura Viva (compose ↔ IaC ↔ diagrama ↔ manifesto, com drift
  check na mesma branch/PR).
- **Revisão adversarial contra a spec** (Tier 2): agente independente parte dos requisitos e
  verifica entrega, registrando achados no plano.
- **Prove-it**: evidência de testes/lint colada no plano como condição de encerramento.
- **Fase 8 com destilação de memória**: promoção de aprendizado a steering/ADR, atualização
  da memória do projeto e arquivamento da spec como delta histórico.
- Repositório de referência: README narrativo com diagrama do ciclo, exemplos guiados
  (Tier 1 e Tier 2), mini-constituição própria e este changelog.

### Modificado

- **Autocontida**: as 8 skills externas que eram dependências viraram tabela de aceleradores
  opcionais; cada fase agora traz instruções completas e critério de saída próprios.
- **Idioma**: skill e documentação reescritas em português do Brasil.
- Erros comuns atualizados (inflação/deflação de tier, pular Fase 0, merge sem destilação,
  revisar o diff em vez da spec).

## [1.0.0] — 2026-06-08 — Versão inicial

### Adicionado

- Fluxo linear de 8 fases (spec → gate de breaking changes → checkpoint humano → plano →
  isolamento → implementação TDD por subagente → verificação e revisão → encerramento), em
  inglês, orquestrando skills do ecossistema Superpowers; endurecida na mesma semana com
  correções em 5 eixos para suporte multi-sessão e agnosticismo de agente.
