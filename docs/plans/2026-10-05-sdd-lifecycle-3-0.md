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

- [x] **T1 (R1, R2, R3).** Na Fase 1 do `SKILL.md`, acrescente:
  - requisitos em linhas `**R<n>** …`, cada um com ao menos um critério GWT
    (`Dado/Quando/Então`) ou EARS (`QUANDO … O SISTEMA DEVE …`), com um exemplo de 3 linhas;
  - o marcador `[ESCLARECER: …]`, a seção `## Esclarecimentos` e a regra de apagar o marcador ao
    receber a resposta;
  - "se houver PRD relacionado em `docs/prd/`, leia-o e cite-o no cabeçalho da spec".

  Na lista de conteúdo mínimo, troque "Requisitos — numerados (R1, R2…), verificáveis" pela forma
  com critério. Conferência: `grep -n 'ESCLARECER\|Esclarecimentos\|docs/prd\|Dado' SKILL.md`
  acha as quatro coisas na Fase 1.
- [x] **T2 (R4).** Na Fase 3, acrescente:
  - "só apresente a spec com zero marcadores `[ESCLARECER` abertos";
  - depois do "sim", gravar `Status: aprovada`, `Aprovado por: <nome>` e
    `Aprovado em: AAAA-MM-DD` no cabeçalho da spec, num commit próprio
    (`docs(specs): aprovar …`).

  Inclua a nota: o registro é feito por quem aprova, ou a pedido explícito dele, porque
  aprovação escrita pelo próprio agente sem pedido não vale. Critério de saída: "aprovação gravada
  no cabeçalho da spec". Conferência: `grep -n 'Aprovado por' SKILL.md` acha a linha na Fase 3 e
  no critério de saída.
- [x] **T3 (R5, R6).** Na Fase 4 (Tier 2), cada tarefa termina com os R<n> que entrega, entre
  parênteses, e um R<n> sem tarefa é lacuna do plano. Na Fase 6, o teste vermelho cita o R<n> no
  nome, na docstring ou num comentário `# cobre: R<n>`; acrescente que citação não prova
  cobertura. Conferência: `grep -n '(R<n>)\|cobre: R' SKILL.md`.
- [x] **T4 (R7).** Na Fase 7 (Tier 2), a revisão adversarial registra a tabela
  `| R | veredito | evidência |`, com o veredito atendido, parcial ou não atendido e a evidência
  em arquivo:linha ou saída de teste. Critério de saída: todo R<n> da spec está na tabela.
  Conferência: `grep -n 'veredito' SKILL.md`.
- [x] **T5 (R8).** Na Fase 8, passo 4 (Destilação), mude o `Status` da spec para `arquivada`.
  Conferência: `grep -n 'arquivada' SKILL.md` acha a linha na Fase 8.
- [x] **T6 (R9).** Em "Erros comuns", acrescente "aprovar só na conversa: sem registro no
  cabeçalho, a retomada não sabe que a spec foi aprovada". Confira que nenhum script do scaffold
  virou obrigatório: `grep -n 'verificar_pr\|scripts/' SKILL.md` só acha uso condicional
  ("se existir"). Confira o tamanho com `wc -l SKILL.md`, que precisa dar 316 ou menos.
- [x] **T7 (R10).** Atualize o README e `docs/exemplos/exemplo-tier-2.md`, e o Tier 1 só se ele
  contradisser a 3.0.0. Inclua o diagrama se ele citar aprovação ou arquivamento.
- [x] **T8 (R10).** Acrescente `[3.0.0] — 2026-10-05 — Requisitos rastreáveis e aprovação
  registrada` no `CHANGELOG.md`, com Adicionado e Modificado, e a nota de que a mudança é major
  pela regra deste repositório.
- [x] **T9.** Rode o prove-it e cole a saída em Evidências:
  - links relativos de `README.md` e `docs/` resolvem: um script `python3 -c` que extrai os
    `](caminho)` e testa `os.path.exists`;
  - os blocos Mermaid estão com sintaxe íntegra, conferidos por leitura;
  - os exemplos ficam coerentes com o fluxo novo.
- [ ] **T10.** Revisão adversarial independente contra a spec, com a tabela por requisito, em
  "Revisão adversarial".
- [ ] **T11.** Merge na `main` e tag `v3.0.0`, só depois do T10 e com a ordem do dono.

## Evidências

```text
$ grep -n "ESCLARECER\|Esclarecimentos\|docs/prd\|Dado" SKILL.md
85:  existentes, padrões do projeto. Se houver PRD relacionado em `docs/prd/`, leia-o e cite-o no
90:  `[ESCLARECER: pergunta]`. Ao receber a resposta, apague o marcador e registre a pergunta e a
91:  resposta na seção `## Esclarecimentos`.
96:    (`Dado/Quando/Então`) ou EARS (`QUANDO … O SISTEMA DEVE …`). Exemplo:
98:    `- Critério: **Dado** 5 falhas seguidas, **Quando** vier a 6ª, **Então** a conta é bloqueada.`
134:- Só apresente a spec com zero marcadores `[ESCLARECER` abertos.

$ grep -n "Aprovado por" SKILL.md
138:- Depois do "sim", grave no cabeçalho da spec `Status: aprovada`, `Aprovado por: <nome>` e
142:**Critério de saída:** aprovação gravada no cabeçalho da spec (`Status`, `Aprovado por` e

$ grep -n "(R<n>)\|cobre: R" SKILL.md
158:  entrega, entre parênteses, `(R<n>)`; requisito sem tarefa é lacuna do plano;
186:     nome, na docstring ou num comentário `# cobre: R<n>`; citação não prova cobertura, só liga o

$ grep -n "veredito" SKILL.md
214:  com uma linha por R<n> na tabela `| R | veredito | evidência |`: veredito `atendido`,
221:justificados por escrito; no Tier 2, todo R<n> da spec está na tabela de vereditos.

$ grep -n "arquivada" SKILL.md
241:   - a spec permanece **arquivada como histórico do delta** — ela não é fonte de verdade viva;
242:     mude o `Status` dela para `arquivada`.

$ grep -n "verificar_pr\|scripts/" SKILL.md
122:   o verificador de drift (ex.: `scripts/verificar_drift_arquitetura.py`) a ser executado antes

$ wc -l SKILL.md  (antes: 276; limite: 316)
     295 SKILL.md

$ python3 -c ... (extrai ](caminho) de README.md, AGENTS.md, CHANGELOG.md e docs/**/*.md; os.path.exists)
(o script ignora o literal "](caminho)", texto descritivo nos planos, não link)
8 links relativos verificados, 0 quebrados

$ mermaid: leitura do bloco do README (rótulos novos nas Fases 3, 7 e 8) e checagem de aspas/colchetes
1 bloco(s) mermaid
aspas/colchetes balanceados em 17 linhas

$ grep de marca de assistente de IA em SKILL.md, README.md, CHANGELOG.md, docs/exemplos (esperado: vazio)
(vazio)

$ coerência dos exemplos: grep -c "R<n>\|\*\*R[0-9]\*\*\|Aprovado por\|veredito\|arquivada" docs/exemplos/exemplo-tier-2.md
8
(R<n> com critério, cabeçalho de aprovação, tabela de veredito, cobre: R e Status arquivada presentes; exemplo-tier-1.md sem spec, não contradiz a 3.0.0, não alterado)
```

## Revisão adversarial: <data> — achados

| R | veredito | evidência |
|---|---|---|
