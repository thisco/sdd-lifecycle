# Spec — Skill sdd-lifecycle 2.0: rigor proporcional por tiers

> **Status:** aprovada · **Tier:** 2 (mudança estrutural do fluxo da skill)
> **Data:** 2026-07-08
> Esta spec descreve o **delta** aplicado sobre a v1, não o sistema inteiro. Após o merge ela é
> histórico arquivado; a fonte de verdade viva é o `SKILL.md`.

## Problema

A skill v1 implementa um pipeline linear de 8 fases aplicado de forma idêntica a toda mudança:
um typo em documentação e uma feature estrutural percorrem o mesmo caminho de spec, checkpoint,
plano e revisão. Isso contradiz o princípio central do SDD 2.0 — **rigor proporcional ao risco**
— e, na prática, incentiva o humano a driblar o processo nas mudanças pequenas, corroendo a
disciplina exatamente onde ela deveria ser barata.

Além disso, a v1:

1. depende de 8 skills do ecossistema de um plugin específico, contradizendo o princípio de
   agnosticismo de ferramenta;
2. não conhece os artefatos de governança do `sdd-scaffold` (constituição `AGENTS.md` + steering
   roteado, `PROJECT_MEMORY.md`, Arquitetura Viva com `mapa.yml` e drift check);
3. revisa o código **contra o diff**, não contra a spec — não pega o requisito silenciosamente
   esquecido;
4. termina no merge, sem destilar o que foi aprendido (memória, steering, ADR);
5. está em inglês, enquanto o ecossistema companion (`sdd-scaffold`) e o público-alvo dos
   artigos são em português.

## Requisitos

### R1 — Fase 0 + matriz de tiers

Nova fase inicial obrigatória (contexto e classificação): ler constituição, ler memória do
projeto, classificar a mudança em Tier 0/1/2 com critérios objetivos. Uma **matriz tier → fases**
define o caminho; as fases são definidas uma única vez, com variações por tier anotadas.
Regras herdadas do SDD 2.0: "na dúvida, tier mais alto" e escalação obrigatória quando um
Tier 0/1 revela impacto estrutural.

### R2 — Scaffold-aware com degradação graciosa

A skill detecta a governança do repositório e a segue: constituição (`AGENTS.md` ou similar),
tabela de roteamento de steering (`docs/steering/`), memória (`docs/PROJECT_MEMORY.md`),
Arquitetura Viva (`Arquitetura/mapa.yml`, `infra/`, script de drift). Quando o repo não tem
esses artefatos, a skill degrada para o fluxo genérico usando os critérios default definidos
nela própria. Nenhum artefato do scaffold é pré-requisito.

### R3 — Autocontida

Cada fase traz instruções completas e critério de saída próprios. Nenhuma skill externa é
dependência; skills disponíveis no agente viram uma tabela opcional de aceleradores.

### R4 — Gate arquitetural ampliado

O gate de breaking changes (Fase 2) passa a cobrir também o checklist de **Impacto
Arquitetural** da Arquitetura Viva (compose ↔ IaC ↔ diagrama ↔ manifesto, na mesma branch/PR,
com drift check quando o script existir).

### R5 — Prove-it e revisão adversarial

Nenhuma tarefa fecha sem **evidência colada no plano** (saída de testes/lint). Em Tier 2, a
revisão é **adversarial contra a spec** (não contra o diff), conduzida por agente/sessão
independente com modelo de raciocínio potente, e registrada no plano em seção própria.

### R6 — Encerramento com destilação

A fase final inclui: `CHANGELOG.md` (Keep a Changelog) antes do merge, decisão PR × merge
direto, destilação de aprendizado (promover a steering/ADR, atualizar `PROJECT_MEMORY.md`)
e arquivamento da spec como delta histórico.

### R7 — Português do Brasil

Skill, README e documentação em PT-BR (jargões técnicos globais consolidados permanecem em
inglês). Referências a modelos continuam por **descritores de capacidade**, sem marca.

### R8 — Repositório de referência pública

O repo `thisco/sdd-lifecycle` contém: `SKILL.md` na raiz (instalável via `git clone` direto no
diretório de skills), README narrativo no padrão do `sdd-scaffold` (problema → modelo → quick
start → instalação por agente), diagrama do ciclo, exemplos guiados fictícios (um Tier 1, um
Tier 2), `LICENSE` MIT, `CHANGELOG.md`, mini-constituição `AGENTS.md`, e specs/planos próprios
em `docs/` — o repo é governado pelo processo que descreve.

## Critérios de aceite

- [ ] Um agente sem nenhum plugin instalado consegue executar o ciclo completo lendo apenas o `SKILL.md`.
- [ ] Um repo criado pelo `sdd-scaffold` tem seus artefatos (steering, memória, mapa) reconhecidos e usados pelas fases correspondentes.
- [ ] Um repo sem scaffold percorre o mesmo ciclo usando os defaults da skill, sem erro nem etapa vazia.
- [ ] Tier 0 termina em commit direto sem gerar spec ou plano.
- [ ] O histórico git do repo mostra a evolução v1 → spec/plano → v2 em commits convencionais.

## Fora de escopo

- Alterar o `sdd-scaffold` (o cross-link no README de lá é follow-up separado).
- Automatizar a instalação da skill (script de setup); a instalação é documentada, não automatizada.
- Tradução paralela para inglês (decisão: PT-BR integral; bilíngue foi descartado pelo custo de manutenção).
- Qualquer conteúdo do projeto de origem: os exemplos guiados usam um projeto fictício.
