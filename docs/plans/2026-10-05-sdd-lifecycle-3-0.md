# Plano: sdd-lifecycle 3.0.0, requisitos rastreáveis e aprovação registrada

> **Tier:** 2 — muda critérios de saída de fases da skill (major por este `AGENTS.md`)
> **Spec relacionada:** `docs/specs/2026-10-05-sdd-lifecycle-3-0.md` (aprovada em 2026-10-05)
> **Branch:** `feat/2026-10-05-spec-verificavel`
> **Data:** 2026-10-05

## Contexto

A spec aprovada pede R1 a R10. Este repositório é só documentação, então o "teste" de cada
tarefa é a conferência objetiva escrita na própria tarefa: um `grep` que precisa achar o texto.
O "prove-it" do repositório completa a verificação: o Mermaid renderiza, os links resolvem e os
exemplos ficam coerentes.

## Arquivos afetados

- `SKILL.md`: Fases 1, 3, 4, 6, 7 e 8, os critérios de saída e "Erros comuns". O arquivo cresce
  no máximo 40 linhas; hoje tem 276.
- `README.md`: a narrativa e o diagrama, onde citam aprovação, revisão e arquivamento.
- `docs/exemplos/exemplo-tier-2.md`: a spec do exemplo com R<n>, critério, cabeçalho de
  aprovação e tabela de veredito.
- `docs/exemplos/exemplo-tier-1.md`: a tarefa do plano leve cita o teste. Muda só se o texto
  atual contradisser a 3.0.0.
- `CHANGELOG.md`: a entrada `[3.0.0]`.

## Impacto Arquitetural

- [x] O compose não muda: N/A, o repositório é de documentação.
- [x] Nenhum módulo de IaC é criado ou alterado: N/A.
- [x] O diagrama Mermaid do README só muda se citar o fluxo alterado; nesse caso, é atualizado
  na mesma branch.

## Tarefas

- [ ] **T1 (R1, R2, R3).** Na Fase 1 do `SKILL.md`, acrescente:
  - requisitos em linhas `**R<n>** …`, cada um com ao menos um critério GWT
    (`Dado/Quando/Então`) ou EARS (`QUANDO … O SISTEMA DEVE …`), com um exemplo de 3 linhas;
  - o marcador `[ESCLARECER: …]`, a seção `## Esclarecimentos` e a regra de apagar o marcador ao
    receber a resposta;
  - "se houver PRD relacionado em `docs/prd/`, leia-o e cite-o no cabeçalho da spec".

  Na lista de conteúdo mínimo, troque "Requisitos — numerados (R1, R2…), verificáveis" pela forma
  com critério. Conferência: `grep -n 'ESCLARECER\|Esclarecimentos\|docs/prd\|Dado' SKILL.md`
  acha as quatro coisas na Fase 1.
- [ ] **T2 (R4).** Na Fase 3, acrescente:
  - "só apresente a spec com zero marcadores `[ESCLARECER` abertos";
  - depois do "sim", gravar `Status: aprovada`, `Aprovado por: <nome>` e
    `Aprovado em: AAAA-MM-DD` no cabeçalho da spec, num commit próprio
    (`docs(specs): aprovar …`).

  Inclua a nota: o registro é feito por quem aprova, ou a pedido explícito dele, porque
  aprovação escrita pelo próprio agente sem pedido não vale. Critério de saída: "aprovação gravada
  no cabeçalho da spec". Conferência: `grep -n 'Aprovado por' SKILL.md` acha a linha na Fase 3 e
  no critério de saída.
- [ ] **T3 (R5, R6).** Na Fase 4 (Tier 2), cada tarefa termina com os R<n> que entrega, entre
  parênteses, e um R<n> sem tarefa é lacuna do plano. Na Fase 6, o teste vermelho cita o R<n> no
  nome, na docstring ou num comentário `# cobre: R<n>`; acrescente que citação não prova
  cobertura. Conferência: `grep -n '(R<n>)\|cobre: R' SKILL.md`.
- [ ] **T4 (R7).** Na Fase 7 (Tier 2), a revisão adversarial registra a tabela
  `| R | veredito | evidência |`, com o veredito atendido, parcial ou não atendido e a evidência
  em arquivo:linha ou saída de teste. Critério de saída: todo R<n> da spec está na tabela.
  Conferência: `grep -n 'veredito' SKILL.md`.
- [ ] **T5 (R8).** Na Fase 8, passo 4 (Destilação), mude o `Status` da spec para `arquivada`.
  Conferência: `grep -n 'arquivada' SKILL.md` acha a linha na Fase 8.
- [ ] **T6 (R9).** Em "Erros comuns", acrescente "aprovar só na conversa: sem registro no
  cabeçalho, a retomada não sabe que a spec foi aprovada". Confira que nenhum script do scaffold
  virou obrigatório: `grep -n 'verificar_pr\|scripts/' SKILL.md` só acha uso condicional
  ("se existir"). Confira o tamanho com `wc -l SKILL.md`, que precisa dar 316 ou menos.
- [ ] **T7 (R10).** Atualize o README e `docs/exemplos/exemplo-tier-2.md`, e o Tier 1 só se ele
  contradisser a 3.0.0. Inclua o diagrama se ele citar aprovação ou arquivamento.
- [ ] **T8 (R10).** Acrescente `[3.0.0] — 2026-10-05 — Requisitos rastreáveis e aprovação
  registrada` no `CHANGELOG.md`, com Adicionado e Modificado, e a nota de que a mudança é major
  pela regra deste repositório.
- [ ] **T9.** Rode o prove-it e cole a saída em Evidências:
  - links relativos de `README.md` e `docs/` resolvem: um script `python3 -c` que extrai os
    `](caminho)` e testa `os.path.exists`;
  - os blocos Mermaid estão com sintaxe íntegra, conferidos por leitura;
  - os exemplos ficam coerentes com o fluxo novo.
- [ ] **T10.** Revisão adversarial independente contra a spec, com a tabela por requisito, em
  "Revisão adversarial".
- [ ] **T11.** Merge na `main` e tag `v3.0.0`, só depois do T10 e com a ordem do dono.

## Evidências

## Revisão adversarial: <data> — achados

| R | veredito | evidência |
|---|---|---|
