# Plano — Skill sdd-lifecycle 2.0

> **Spec:** `docs/specs/2026-07-08-sdd-lifecycle-2-0.md` · **Tier:** 2 · **Branch:** `feat/2026-07-08-sdd-lifecycle-2-0`

## Impacto Arquitetural

- [ ] O `docker-compose.yml` foi alterado? **Não** — repositório de documentação, sem infra local.
- [ ] Algum módulo de IaC foi criado ou alterado? **Não.**
- [ ] Diagrama e manifesto de arquitetura foram atualizados? **N/A** — o diagrama do ciclo vive no README (Mermaid) e é criado nesta entrega.

## Tarefas

- [x] **T1 — Reescrever `SKILL.md` no modelo 2.0** (R1–R7): Fase 0 + matriz de tiers, fases
  autocontidas com critério de saída, detecção scaffold-aware com degradação graciosa, gate
  arquitetural ampliado (Arquitetura Viva), revisão adversarial contra a spec, encerramento com
  destilação, retomada multi-sessão e erros comuns atualizados, tabela de aceleradores opcionais.
  Em português.
- [x] **T2 — README narrativo** (R8): problema → modelo → diagrama Mermaid do ciclo → quick
  start/instalação por agente → uso → exemplos → relação com o `sdd-scaffold` → referências →
  licença → contribuindo.
- [x] **T3 — Exemplos guiados** (R8): `docs/exemplos/exemplo-tier-1.md` (bug fix com plano leve
  e evidência) e `docs/exemplos/exemplo-tier-2.md` (feature com spec, gate, checkpoint e revisão
  adversarial com achado). Projeto fictício.
- [x] **T4 — Governança do próprio repo** (R8): `AGENTS.md` mini-constituição + `CHANGELOG.md`
  com as entradas 1.0.0 e 2.0.0.
- [x] **T5 — Verificação e evidência**: validar o Mermaid, conferir links internos, colar
  evidências neste plano e registrar a revisão adversarial contra a spec.
- [ ] **T6 — Publicação**: merge `--no-ff` na `main`, criar `thisco/sdd-lifecycle` público no
  GitHub, push, e apontar a skill local (`~/.claude/skills/sdd-lifecycle`) para o clone.

## Plano de testes

Repositório de documentação — a "suíte" é verificação de artefato:

1. Render do diagrama Mermaid sem erro de sintaxe (validação com parser local ou preview).
2. Todos os links relativos do README e dos exemplos resolvem para arquivos existentes.
3. Critérios de aceite da spec percorridos um a um (checklist na revisão adversarial).

## Checkpoints

- Spec aprovada pelo humano antes deste plano (feito — aprovação registrada na sessão de design).
- Revisão adversarial registrada abaixo antes do merge.

## Evidências

**Diagrama Mermaid** — validado com o parser oficial (`mermaid@11` + jsdom):

```
$ node -e "… mermaid.parse(src) …"
MERMAID OK — tipo: flowchart-v2
```

**Links relativos** — varredura de todos os `](caminho)` não-http em README, AGENTS, SKILL,
exemplos, specs e planos:

```
$ for f in README.md AGENTS.md SKILL.md docs/**/*.md; do …verificar existência… done
verificacao de links concluida   # zero links quebrados
```

## Revisão adversarial: 2026-07-08 — achados

Passe requisito a requisito da spec contra os artefatos entregues:

- **R1** (Fase 0 + matriz) ✅ — Fase 0 com constituição/memória/tier, matriz tier → fases,
  "na dúvida, tier mais alto" e escalação obrigatória presentes no `SKILL.md`.
- **R2** (scaffold-aware) ✅ — detecção em Fase 0 (steering), Fase 2 (mapa/drift, com "N/A e
  siga" para repos sem os artefatos) e Fase 8 (memória condicional).
- **R3** (autocontida) ✅ — toda fase tem instruções e critério de saída próprios; skills
  externas rebaixadas a tabela de aceleradores opcionais.
- **R4** (gate ampliado) ✅ — Fase 2 cobre destrutivas + checklist de Arquitetura Viva na
  mesma branch/PR.
- **R5** (prove-it + adversarial) ✅ — Fase 7; evidências e seção de registro no plano.
- **R6** (destilação) ✅ — Fase 8, incluindo arquivamento da spec como delta.
- **R7** (PT-BR, descritores de capacidade) ✅ — nenhuma marca de modelo/assistente no
  `SKILL.md` ou artefatos.
- **R8** (repo de referência) ⚠️ **ACHADO 1 (médio, corrigido):** a instrução de instalação
  como *skill de projeto* mandava clonar o repo para dentro de `.claude/skills/`, criando um
  repo git aninhado que o git do projeto trata como submódulo não registrado. Corrigido no
  README: cópia do `SKILL.md` (ou submódulo explícito).
- **Critérios de aceite** — percorridos um a um: agente sem plugin executa só com o
  `SKILL.md` ✅; artefatos do scaffold reconhecidos nas fases correspondentes ✅; repo sem
  scaffold percorre o ciclo com defaults ✅; Tier 0 termina em commit direto ✅; histórico git
  registra v1 → spec/plano → v2 em commits convencionais ✅.
