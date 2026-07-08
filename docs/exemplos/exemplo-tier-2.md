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

## Problema
O time de facilities consolida ocupação manualmente, copiando dados da UI.

## Requisitos
- R1 — `GET /v1/reservas/exportar?inicio=YYYY-MM-DD&fim=YYYY-MM-DD` retorna CSV.
- R2 — Colunas: sala, responsável, início, fim, status — horários no fuso do solicitante.
- R3 — Restrito ao papel `gestor-facilities` (RBAC existente).
- R4 — Período máximo de 92 dias; acima disso, HTTP 422 com mensagem clara.

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

Usuário responde: *"Aprovo, mas o limite é 92 dias corridos incluindo as duas pontas."* A spec
é ajustada (R4 reescrito) e o fluxo segue.

## Fase 4 — Plano completo

`docs/plans/2026-07-08-exportacao-csv-reservas.md` com: arquivos e assinaturas
(`exportar_reservas_csv(inicio: date, fim: date, fuso: ZoneInfo) -> Iterator[str]`), plano de
testes por tarefa, checkboxes, seção Impacto Arquitetural (N/A justificado) e a tarefa final
de CHANGELOG.

## Fases 5 e 6 — Isolamento e implementação TDD

Branch `feat/2026-07-08-exportacao-csv-reservas`. A implementação é delegada a um subagente com
**modelo eficiente**, que percorre as tarefas em ciclos vermelho → verde → refatora, marca os
checkboxes e roda `pytest -q` + `ruff check` a cada tarefa.

## Fase 7 — Verificação e revisão adversarial

Evidências (suíte completa e lint) coladas no plano. Em seguida, uma sessão **independente**
com **modelo de raciocínio potente** revisa **contra a spec**, requisito a requisito:

```markdown
## Revisão adversarial: 2026-07-08 — achados

- R1 ✅ endpoint entregue conforme contrato.
- R2 ⚠️ ACHADO 1 (crítico): horários exportados em UTC — o parâmetro de fuso existe na
  assinatura, mas nunca é aplicado na serialização. A suíte não pegou porque o teste usa
  fuso UTC. Requisito NÃO atendido.
- R3 ✅ RBAC verificado no endpoint (teste de 403 presente).
- R4 ✅ limite de 92 dias corridos incluindo as pontas (teste de borda com 92 e 93 dias).
- Threat-model ⚠️ ACHADO 2 (médio): sanitização de fórmula cobre `=` mas não `+`, `-`, `@`.
```

É exatamente o tipo de defeito que revisão de diff não pega: o código novo é internamente
consistente — ele apenas **não faz o que a spec pediu**. Os dois achados voltam à Fase 6
(críticos, não arquiteturais), o subagente escreve primeiro os testes que os expõem, corrige,
e a revisão é re-executada: limpa.

## Fase 8 — Encerramento e destilação

1. `CHANGELOG.md` (1.5.0 — Adicionado) na branch, antes do merge.
2. PR com testes verdes; merge; branch apagada.
3. **Destilação:**
   - A regra "toda exportação de CSV sanitiza fórmulas em todos os prefixos perigosos" é
     **promovida ao steering** (`docs/steering/seguranca.md`) — deixou de ser conhecimento da
     spec para virar norma permanente.
   - `PROJECT_MEMORY.md` ganha: *"testes de fuso devem usar fuso ≠ UTC — bug de R2 passou
     porque o teste usava UTC"*.
   - A spec é arquivada como histórico do delta.

**O que este exemplo demonstra:** os dois checkpoints humanos aconteceram onde errar era caro
(spec e limite de negócio), a revisão adversarial pegou um requisito silenciosamente não
atendido, e o projeto saiu **mais governado** do que entrou — uma norma nova no steering e um
gotcha novo na memória.
