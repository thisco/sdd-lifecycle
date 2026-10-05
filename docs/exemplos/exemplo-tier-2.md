# Exemplo guiado — Tier 2: feature com spec, gate e revisão adversarial

Mesmo projeto fictício do [exemplo Tier 1](exemplo-tier-1.md): **agenda-api** (Python 3.12 +
FastAPI + pytest), governado por `AGENTS.md` + steering.

**Pedido do usuário:**

> Use a `sdd-lifecycle` para implementar a exportação CSV das reservas por período, para o
> time de facilities consolidar ocupação das salas.

---

## Fase 0 — Contexto e classificação

Constituição e steering lidos; memória consultada. Classificação anunciada:

> **Tier 2** — feature nova com contrato novo (endpoint + formato de arquivo). Caminho
> completo: Fase 0 → 1 → 2 → 3 → 4 → 5 → 6 → 7 (adversarial) → 8.

## Fase 1 — Especificação (spec como delta)

O agente explora o código (rotas existentes, serviço de reservas, como auth é aplicada) e faz
**uma pergunta por vez** ao usuário — ex.: *"O CSV deve respeitar o fuso do solicitante ou
padronizar em UTC?"* (resposta: fuso do solicitante). Depois redige
`docs/specs/2026-07-08-exportacao-csv-reservas.md`:

```markdown
# Spec — Exportação CSV de reservas por período

> **Tier:** 2
> **Status:** rascunho
> **PRD:** `docs/prd/facilities-ocupacao.md` (lido na Fase 1)

## Problema
O time de facilities consolida ocupação manualmente, copiando dados da UI.

## Requisitos
**R1** `GET /v1/reservas/exportar?inicio=YYYY-MM-DD&fim=YYYY-MM-DD` retorna CSV.
- Critério: **Dado** um período válido, **Quando** o gestor chama o endpoint, **Então** a
  resposta é `text/csv`.

**R2** Colunas: sala, responsável, início, fim, status — horários no fuso do solicitante.
- Critério: **Dado** um solicitante em America/Sao_Paulo, **Quando** exporta uma reserva às
  21:30 locais, **Então** o CSV mostra 21:30, não o horário em UTC.

**R3** Restrito ao papel `gestor-facilities` (RBAC existente).
- Critério: **Dado** um usuário sem o papel, **Quando** chama o endpoint, **Então** recebe 403.

**R4** Período máximo de 92 dias; acima disso, HTTP 422 com mensagem clara.
- Critério: **Dado** um período de 93 dias, **Quando** chama o endpoint, **Então** recebe 422.

## Esclarecimentos
- P: O CSV deve respeitar o fuso do solicitante ou padronizar em UTC? → R: fuso do solicitante.

## Critérios de aceite
- [ ] CSV abre no Excel/LibreOffice sem ajuste manual (UTF-8 com BOM).
- [ ] Usuário sem o papel recebe 403.
- [ ] Período de 93 dias recebe 422.

## Threat-model (entrada externa)
- Injeção de fórmula em CSV (`=`, `+`, `-`, `@` no início de célula) → prefixar com `'`.
- Enumeração de reservas alheias → RBAC no endpoint, não só na UI.

## Fora de escopo
- Exportação XLSX; agendamento de relatórios recorrentes.
```

## Fase 2 — Gate arquitetural

1. **Mudanças destrutivas:** nenhuma — endpoint novo, nenhum contrato existente alterado.
   Sem ADR obrigatória.
2. **Impacto Arquitetural:** compose **não** muda (sem serviço novo); IaC **não** muda;
   diagrama **não** muda (o endpoint vive no componente API já desenhado). Registrado "N/A"
   no plano.

## Fase 3 — Checkpoint humano

> "Você aprova esta especificação, ou há regras de negócio/técnicas a ajustar antes do plano?"

A spec só foi apresentada com zero marcadores `[ESCLARECER` abertos. O usuário responde:
*"Aprovo, mas o limite é 92 dias corridos incluindo as duas pontas."* A ressalva muda um
requisito, então a spec é ajustada (R4 reescrito) e **apresentada de novo**, com o mesmo
pedido de aprovação. Só depois do "sim" à versão ajustada o cabeçalho é gravado num commit
próprio (`docs(specs): aprovar exportacao-csv-reservas`):

```markdown
> **Status:** aprovada
> **Aprovado por:** Marina Costa
> **Aprovado em:** 2026-07-08
```

## Fase 4 — Plano completo

`docs/plans/2026-07-08-exportacao-csv-reservas.md` com: arquivos e assinaturas
(`exportar_reservas_csv(inicio: date, fim: date, fuso: ZoneInfo) -> Iterator[str]`), plano de
testes por tarefa, checkboxes, seção Impacto Arquitetural (N/A justificado) e a tarefa final
de CHANGELOG. Cada tarefa termina com os requisitos que entrega, por exemplo
`- [ ] T2 — serialização no fuso do solicitante (R2)`; todo R<n> tem tarefa.

## Fases 5 e 6 — Isolamento e implementação TDD

Branch `feat/2026-07-08-exportacao-csv-reservas`. A implementação é delegada a um subagente com
**modelo eficiente**, que percorre as tarefas em ciclos vermelho → verde → refatora, marca os
checkboxes e roda `pytest -q` + `ruff check` a cada tarefa. Cada teste vermelho cita o
requisito (`# cobre: R3`).

## Fase 7 — Verificação e revisão adversarial

Evidências (suíte completa e lint) coladas no plano. Em seguida, uma sessão **independente**
com **modelo de raciocínio potente** revisa **contra a spec**, requisito a requisito:

```markdown
## Revisão adversarial: 2026-07-08 — achados

| R | veredito | evidência |
|---|---|---|
| R1 | atendido | `src/rotas/reservas.py:41`; `test_exportar_retorna_csv` passa |
| R2 | não atendido | `src/servicos/exportacao.py:27`: o fuso chega na assinatura e não é aplicado |
| R3 | atendido | `test_exportar_sem_papel_retorna_403` passa |
| R4 | atendido | `tests/test_exportacao.py:58`: borda com 92 e 93 dias |

- ACHADO 1 (crítico, R2): horários exportados em UTC. A suíte não pegou porque o teste usa
  fuso UTC. Requisito NÃO atendido.
- ACHADO 2 (médio, threat-model): sanitização de fórmula cobre `=` mas não `+`, `-`, `@`.
```

É exatamente o tipo de defeito que revisão de diff não pega: o código novo é internamente
consistente — ele apenas **não faz o que a spec pediu**. Os dois achados voltam à Fase 6
(críticos, não arquiteturais), o subagente escreve primeiro os testes que os expõem, corrige,
e a revisão é re-executada: limpa, com R2 em `atendido` e todos os R<n> na tabela.

## Fase 8 — Encerramento e destilação

1. `CHANGELOG.md` (1.5.0 — Adicionado) na branch, antes do merge, e a spec muda para
   `Status: arquivada` no mesmo commit.
2. PR com testes verdes; merge; branch apagada.
3. **Destilação:**
   - A regra "toda exportação de CSV sanitiza fórmulas em todos os prefixos perigosos" é
     **promovida ao steering** (`docs/steering/seguranca.md`) — deixou de ser conhecimento da
     spec para virar norma permanente.
   - `PROJECT_MEMORY.md` ganha: *"testes de fuso devem usar fuso ≠ UTC — bug de R2 passou
     porque o teste usava UTC"*.
   - A spec fica como histórico do delta, já arquivada no passo 1.

**O que este exemplo demonstra:** os dois checkpoints humanos aconteceram onde errar era caro
(spec e limite de negócio), a revisão adversarial pegou um requisito silenciosamente não
atendido, e o projeto saiu **mais governado** do que entrou — uma norma nova no steering e um
gotcha novo na memória.
