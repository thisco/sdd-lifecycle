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
(saída real e justificativa em "Correções da revisão", item 6; a saída inicial vazia estava errada)

$ coerência dos exemplos: grep -c "R<n>\|\*\*R[0-9]\*\*\|Aprovado por\|veredito\|arquivada" docs/exemplos/exemplo-tier-2.md
8
(R<n> com critério, cabeçalho de aprovação, tabela de veredito, cobre: R e Status arquivada presentes; exemplo-tier-1.md sem spec, não contradiz a 3.0.0, não alterado)
```

### Correções da revisão

```text
$ wc -l SKILL.md  (limite: 316)
301 SKILL.md

# 1
$ grep -n "veredito por" README.md
57:veredito por requisito (`| R | veredito | evidência |`). Cada requisito `R<n>` tem critério de
87:    F6 --> F7["Fase 7 — Verificação<br/>evidência no plano · Tier 2: revisão adversarial, veredito por requisito"]
# 2
$ grep -n "rascunho" SKILL.md
201:     `rascunho` e repita o checkpoint da Fase 3, gravando a nova aprovação, antes de voltar a
# 3
$ grep -n "arquivada" SKILL.md
233:1. **CHANGELOG e spec antes do merge.** No Tier 2, mude o `Status` da spec para `arquivada`
245:   - a spec permanece **arquivada como histórico do delta** (passo 1) — ela não é fonte de
249:**Critério de saída:** merge concluído, changelog registrado, spec arquivada, aprendizado
# 4
$ grep -n "requisitos R<n> com critério" SKILL.md
105:**Critério de saída:** spec redigida cobrindo problema, requisitos R<n> com critério, aceite e
# 5
$ grep -n "opcional" SKILL.md
99:  - **Critérios de aceite** — checklist objetivo, opcional: complementa os critérios de cada
# 6 (busca de marca, saída real)
$ grep -rniE 'claude|anthropic|gpt|openai|gemini|copilot' SKILL.md README.md CHANGELOG.md docs/exemplos
SKILL.md:44:   `CLAUDE.md`, `GEMINI.md`, outro arquivo de convenções apontado pelo README. Leia-o por
README.md:97:**Claude Code (skill pessoal):**
README.md:100:git clone https://github.com/thisco/sdd-lifecycle.git ~/.claude/skills/sdd-lifecycle
README.md:103:**Claude Code (skill de projeto, versionada com o repo):** clonar um repo git dentro do seu
README.md:109:  && mkdir -p .claude/skills/sdd-lifecycle \
README.md:110:  && cp /tmp/sdd-lifecycle/SKILL.md .claude/skills/sdd-lifecycle/
README.md:113:**Gemini CLI, Copilot e outros agentes:** referencie o `SKILL.md` como instrução de contexto —
README.md:114:por exemplo, importando-o no arquivo de instruções do agente (`GEMINI.md`,
README.md:115:`.github/copilot-instructions.md`) ou colando o caminho no prompt:
README.md:120:Para atualizar: `git -C ~/.claude/skills/sdd-lifecycle pull`.
README.md:185:- [Anthropic Engineering — Claude Code best practices](https://www.anthropic.com/engineering/claude-code-best-practices) — práticas de engenharia com agentes.
README.md:186:- [Anthropic Engineering — Context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — engenharia de contexto para agentes.
# 7
$ grep -n "apresentada de novo" docs/exemplos/exemplo-tier-2.md
82:requisito, então a spec é ajustada (R4 reescrito) e **apresentada de novo**, com o mesmo
# 8
$ grep -n "ESCLARECER" SKILL.md | tail -1
262:   Confira o `Status` da spec e se há marcadores `[ESCLARECER` abertos: sem `aprovada`, volte
# Coerência de Status (conferência)
$ wc -l SKILL.md
301 SKILL.md
# 1 retomada
$ grep -n "retome a Fase 8" SKILL.md
263:   volte à Fase 3; `arquivada` → retome a Fase 8; `aprovada` → siga do plano.
# 2 escalação
$ grep -n "escalação da Fase 7\|volta a .rascunho" SKILL.md
202:     codar. O mesmo vale ao reabrir a spec por escalação da Fase 7.
220:  escalam à Fase 1, e o `Status` da spec volta a `rascunho`. Feedback tecnicamente questionável se discute com evidência, não se
# 3 critério da Fase 8
$ grep -n "spec arquivada (Tier 2)" SKILL.md
249:**Critério de saída:** merge concluído, changelog registrado, spec arquivada (Tier 2),
```

Justificativa do item 6: as menções de harness ficam na seção de instalação e nas referências do
README, e SKILL.md:44 cita nomes de arquivos de convenção (`CLAUDE.md`, `GEMINI.md`). Todas
são pré-existentes na `main` e necessárias para dizer onde instalar a skill e qual arquivo de
regras procurar; nenhuma é marca de assistente em artefato gerado.

## Revisão adversarial: 2026-10-05 — achados

Revisor independente, sessão nova, modelo de raciocínio potente, contra a spec.

| R | veredito | evidência |
|---|---|---|
| R1 | atendido | SKILL.md:95-98 |
| R2 | atendido | SKILL.md:89-91 |
| R3 | atendido | SKILL.md:84-86 |
| R4 | atendido | SKILL.md:134-143 |
| R5 | atendido | SKILL.md:156-158 |
| R6 | atendido | SKILL.md:185-187 |
| R7 | atendido | SKILL.md:213-215, 221 |
| R8 | atendido | SKILL.md:241-242 |
| R9 | atendido | SKILL.md:23-24, 122 (script só como "ex.:", condicional) |
| R10 | parcial | README.md:87 (Mermaid conferido só por leitura); tag pendente, esperado |

Achados e tratamento:

1. Médio. O rótulo Mermaid `veredito por R<n>`, em README.md:87, perde o `<n>` no sanitizador.
   Tratamento: trocar por texto sem `<>`.
2. Médio. Quando a escalação ou o strict fallback voltam à Fase 1, o cabeçalho fica com
   `Status: aprovada` obsoleto (SKILL.md:194-199, 216-217). Tratamento: ao reabrir a spec, o
   `Status` volta a rascunho, e a nova aprovação é gravada na Fase 3.
3. Médio. A spec só é arquivada depois do merge (SKILL.md:235-246), e o critério de saída da Fase
   8 não cita o arquivamento. Tratamento: arquivar antes do merge, junto do CHANGELOG, e citar
   isso no critério de saída.
4. Médio. CHANGELOG.md:28 afirma que o critério de saída da Fase 1 mudou, e ele não mudou.
   Tratamento: incluir "requisitos R<n> com critério" no critério de saída da Fase 1.
5. Baixo. Checklist de aceite e critério por R<n> convivem de forma ambígua (SKILL.md:99).
   Tratamento: o checklist é opcional e complementa.
6. Baixo. A evidência de "marca de IA" registra vazio, mas o README cita harnesses na seção de
   instalação, menções que já existiam. Tratamento: registrar a saída real e a exceção.
7. Baixo. No exemplo Tier 2, a aprovação com ressalva é gravada sem nova apresentação
   (exemplo-tier-2.md:80-82). Tratamento: mostrar a reapresentação.
8. Baixo. A retomada multi-sessão não confere `Status` nem marcadores abertos (SKILL.md:252-258).
   Tratamento: acrescentar o passo.

Veredito do revisor: não vai a merge antes de tratar os achados 1 a 4. Todos os 8 serão
tratados.
