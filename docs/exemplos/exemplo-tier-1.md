# Exemplo guiado — Tier 1: bug fix com plano leve

Projeto fictício: **agenda-api**, uma API de reserva de salas (Python 3.12 + FastAPI + pytest),
com `AGENTS.md` na raiz e steering em `docs/steering/`.

**Pedido do usuário:**

> Use a `sdd-lifecycle` para corrigir o bug das reservas noturnas: reservas criadas depois das
> 21h estão sendo gravadas no dia seguinte.

---

## Fase 0 — Contexto e classificação

O agente:

1. Lê `AGENTS.md` (constituição) e, pela tabela de roteamento, `docs/steering/sdd-processo.md`
   (vai alterar código) — descobre que a suíte roda com `pytest -q` e o linter é `ruff`.
2. Lê `docs/PROJECT_MEMORY.md` — encontra a nota: *"o banco grava timestamps em UTC; a
   conversão para o fuso do usuário acontece só na borda da API"*. Isso já sugere a causa.
3. Reformula o pedido: *"Reservas com horário local ≥ 21h são persistidas com data errada
   porque a conversão para UTC acontece antes da extração da data."*
4. Classifica e anuncia:

> **Tier 1** — bug fix localizado; a investigação aponta um único ponto de conversão em
> `servicos/reservas.py`, sem mudança de contrato. Caminho: Fase 0 → 4 (plano leve) → 5 → 6 → 7 → 8.

## Fase 4 — Plano leve

Arquivo `docs/plans/2026-07-08-fix-data-reserva-noturna.md`:

```markdown
# Plano leve — Reservas noturnas gravadas no dia seguinte

**Contexto:** `criar_reserva()` converte o horário local para UTC e só depois extrai a data
do dia. Para horários locais ≥ 21h (UTC-3), a conversão cruza a meia-noite em UTC e a data
extraída avança um dia.

**Arquivos:**
- `src/servicos/reservas.py` — extrair a data ANTES da conversão para UTC.
- `tests/servicos/test_reservas.py` — novo teste de regressão.

**Teste que prova o fix:** `test_reserva_as_21h_mantem_data_local` — cria reserva às 21:30
America/Sao_Paulo e afirma que `reserva.data == date(2026, 7, 8)` (hoje, não amanhã).

## Impacto Arquitetural
- [ ] compose alterado? **Não**
- [ ] módulo IaC criado/alterado? **Não**
- [ ] diagrama/manifesto atualizados? **Não** — sem mudança estrutural.

## Tarefas
- [ ] T1 — teste de regressão (vermelho)
- [ ] T2 — fix mínimo (verde) + suíte/lint
- [ ] T3 — CHANGELOG + evidências

## Evidências
_(a preencher)_
```

## Fase 5 — Isolamento

```bash
git checkout main && git pull
git checkout -b fix/2026-07-08-data-reserva-noturna
```

## Fase 6 — Implementação TDD

**Vermelho** — o teste novo primeiro, rodado para vê-lo falhar pelo motivo certo:

```
$ pytest tests/servicos/test_reservas.py::test_reserva_as_21h_mantem_data_local -q
F
AssertionError: assert datetime.date(2026, 7, 9) == datetime.date(2026, 7, 8)
1 failed in 0.21s
```

**Verde** — o fix mínimo (extração da data movida para antes da conversão), e a suíte inteira:

```
$ pytest -q
124 passed in 3.87s
$ ruff check src tests
All checks passed!
```

Checkboxes T1 e T2 marcados no plano.

## Fase 7 — Verificação

Saídas acima **coladas na seção "Evidências" do plano** (prove-it). Revisão própria contra o
plano leve: o teste prometido existe, falhou antes do fix e passa depois — o fix está provado.

## Fase 8 — Encerramento e destilação

1. `CHANGELOG.md` na branch, antes do merge:

   ```markdown
   ## [1.4.2] — 2026-07-08 — Correção de data em reservas noturnas
   ### Corrigido
   - Reservas com horário local a partir das 21h eram gravadas no dia seguinte
     (extração de data após conversão UTC).
   ```

2. Commits convencionais:

   ```
   test(reservas): cobrir reserva noturna que cruza meia-noite em UTC
   fix(reservas): extrair data local antes da conversao para UTC
   docs(changelog): registrar 1.4.2
   ```

3. PR aberto (repo compartilhado), merge após CI verde, branch apagada.
4. **Destilação:** a nota da memória já cobria a regra de UTC; o agente acrescenta uma linha ao
   `PROJECT_MEMORY.md`: *"extração de data deve sempre preceder conversão de fuso — regressão
   corrigida em 1.4.2"*.

**Custo total de processo:** um plano de uma página e três commits. Rigor proporcional: nenhum
checkpoint humano foi necessário porque nenhuma decisão irreversível foi tomada.
