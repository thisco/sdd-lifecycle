# Plano — Skill sdd-lifecycle 2.0

> **Spec:** `docs/specs/2026-07-08-sdd-lifecycle-2-0.md` · **Tier:** 2 · **Branch:** `feat/2026-07-08-sdd-lifecycle-2-0`

## Impacto Arquitetural

- [ ] O `docker-compose.yml` foi alterado? **Não** — repositório de documentação, sem infra local.
- [ ] Algum módulo de IaC foi criado ou alterado? **Não.**
- [ ] Diagrama e manifesto de arquitetura foram atualizados? **N/A** — o diagrama do ciclo vive no README (Mermaid) e é criado nesta entrega.

## Tarefas

- [ ] **T1 — Reescrever `SKILL.md` no modelo 2.0** (R1–R7): Fase 0 + matriz de tiers, fases
  autocontidas com critério de saída, detecção scaffold-aware com degradação graciosa, gate
  arquitetural ampliado (Arquitetura Viva), revisão adversarial contra a spec, encerramento com
  destilação, retomada multi-sessão e erros comuns atualizados, tabela de aceleradores opcionais.
  Em português.
- [ ] **T2 — README narrativo** (R8): problema → modelo → diagrama Mermaid do ciclo → quick
  start/instalação por agente → uso → exemplos → relação com o `sdd-scaffold` → referências →
  licença → contribuindo.
- [ ] **T3 — Exemplos guiados** (R8): `docs/exemplos/exemplo-tier-1.md` (bug fix com plano leve
  e evidência) e `docs/exemplos/exemplo-tier-2.md` (feature com spec, gate, checkpoint e revisão
  adversarial com achado). Projeto fictício.
- [ ] **T4 — Governança do próprio repo** (R8): `AGENTS.md` mini-constituição + `CHANGELOG.md`
  com as entradas 1.0.0 e 2.0.0.
- [ ] **T5 — Verificação e evidência**: validar o Mermaid, conferir links internos, colar
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

_(a preencher na T5)_

## Revisão adversarial: _(pendente)_
