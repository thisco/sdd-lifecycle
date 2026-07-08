# AGENTS.md — Constituição do Repositório

Regras para humanos e agentes de IA contribuindo neste repositório. Este repo contém uma
**skill de processo** (`SKILL.md`) e sua documentação — não há código de aplicação. A
governança abaixo é a própria skill aplicada a um repo de documentação.

## Princípios

1. **O repo pratica o que prega.** Toda mudança segue o ciclo do `SKILL.md`, classificada por
   tier: typo/ajuste de redação = Tier 0 (commit direto); reorganização de seção ou exemplo
   novo = Tier 1 (plano leve em `docs/plans/`); mudança no **fluxo da skill** (fases, matriz,
   gates, critérios) = Tier 2 (spec em `docs/specs/` → checkpoint humano → plano → revisão
   adversarial). Na dúvida, tier mais alto.
2. **Agnosticismo de ferramenta.** Nenhum artefato, commit ou exemplo menciona marca de
   assistente de IA. Modelos são referidos por descritores de capacidade.
3. **Português do Brasil** em toda a documentação; jargões técnicos globais consolidados
   permanecem em inglês.
4. **Autocontenção é requisito, não estilo.** O `SKILL.md` não pode adquirir dependência de
   skill, plugin ou plataforma específica. Integrações viram "aceleradores opcionais".
5. **Compatibilidade com o companion.** Mudanças no fluxo devem permanecer coerentes com os
   artefatos do [`sdd-scaffold`](https://github.com/thisco/sdd-scaffold) (constituição,
   steering, memória, Arquitetura Viva) — e continuar degradando graciosamente sem eles.

## Convenções

- **Commits:** Conventional Commits (`feat:`, `fix:`, `docs:`, `docs(specs):` …), em PT-BR.
- **Branches:** `feat|fix/YYYY-MM-DD-nome-curto` a partir da `main`.
- **CHANGELOG:** Keep a Changelog, versão semântica incrementada **antes do merge**, na branch.
  Mudança de fluxo da skill = versão **major**; exemplos/README = minor; correções = patch.
- **Verificação (o "prove-it" deste repo):** diagrama Mermaid renderiza sem erro; links
  relativos resolvem; exemplos permanecem consistentes com o fluxo do `SKILL.md`.

## Mapa do repositório

```
SKILL.md          # a skill — fonte de verdade viva do processo
README.md         # narrativa pública: problema → modelo → instalação
docs/
├── specs/        # deltas de mudança do fluxo (histórico arquivado após merge)
├── plans/        # planos com checkboxes, evidências e revisões adversariais
└── exemplos/     # walkthroughs fictícios (Tier 1 e Tier 2)
```
