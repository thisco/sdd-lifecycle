# Spec: sdd-lifecycle 3.0.0, requisitos rastreáveis e aprovação registrada

> **Tier:** 2 (muda critérios de saída das Fases 1, 3, 4, 6, 7 e 8; por este `AGENTS.md`, major).
> **Status:** arquivada
> **Aprovado por:** thiago
> **Aprovado em:** 2026-10-05
> **Data:** 2026-10-05
> **Companheira:** spec do sdd-scaffold 1.7.0 (`docs/specs/2026-10-05-spec-verificavel-e-aprovada.md`
> naquele repositório). Origem: análise da prática SDD da curadoria A.5, §7 "Release 1".

## Motivação

1. **A rastreabilidade não sai do texto.** A skill pede requisitos `R1, R2…`, mas não liga cada
   um a uma tarefa, a um teste nem ao veredito da revisão. A revisão adversarial parte dos
   requisitos e não deixa registro por requisito.
2. **O registro da aprovação não tem lugar definido.** A Fase 3 exige "aprovação humana
   registrada" e não diz onde gravá-la.
3. **A dúvida pendente fica só na conversa.** Ela se perde entre sessões, e a retomada
   multi-sessão depende do arquivo.
4. **A spec não é arquivada.** A Fase 8 diz que ela é arquivada, mas não manda mudar o `Status`.
5. **O PRD fica de fora.** O `AGENTS.md` do scaffold exige PRD em Tier 2, e a skill nunca o
   cita.

## Requisitos

**R1** A Fase 1 manda numerar os requisitos em linhas `**R<n>** …`, cada uma com ao menos um
critério de aceite em GWT ou EARS.
- Critério: **Dado** o `SKILL.md`, **Quando** se lê a Fase 1, **Então** há a instrução e um
  exemplo curto de R<n> com critério.

**R2** A Fase 1 manda marcar no arquivo, com `[ESCLARECER: …]`, toda dúvida que o usuário ainda
não respondeu. Ao receber a resposta, o marcador é apagado e a pergunta e a resposta vão para
`## Esclarecimentos`.
- Critério: **Dado** a Fase 1, **Quando** lida, **Então** o marcador, a seção e a regra de
  remoção estão descritos.

**R3** A Fase 1 manda ler o PRD relacionado, quando houver um em `docs/prd/`, e citá-lo no
cabeçalho da spec. Sem PRD, a fase segue.
- Critério: **Dado** a Fase 1, **Quando** lida, **Então** a leitura condicional do PRD aparece.

**R4** A Fase 3 só apresenta a spec com zero marcadores abertos. Depois do "sim", a fase grava
três campos no cabeçalho, num commit próprio: `Status: aprovada`, `Aprovado por` e
`Aprovado em`. O critério de saída passa a ser "aprovação gravada no cabeçalho".
- Critério: **Dado** a Fase 3, **Quando** lida, **Então** o critério de saída cita os três
  campos.

**R5** A Fase 4 manda terminar cada tarefa com os R<n> que ela entrega, entre parênteses. Um
requisito sem tarefa é lacuna do plano.
- Critério: **Dado** a Fase 4, **Quando** lida, **Então** a regra `(R<n>)` aparece.

**R6** A Fase 6 manda que o teste vermelho cite o R<n> no nome, na docstring ou em comentário
(`# cobre: R<n>`).
- Critério: **Dado** a Fase 6, **Quando** lida, **Então** a convenção aparece com o limite
  "citação não prova cobertura".

**R7** A Fase 7, no Tier 2, registra uma linha por R<n> na tabela `| R | veredito | evidência |`.
O veredito é atendido, parcial ou não atendido, e a evidência é arquivo:linha ou saída de teste.
O critério de saída exige todos os R<n> na tabela.
- Critério: **Dado** a Fase 7, **Quando** lida, **Então** a tabela e o critério de saída estão
  lá.

**R8** A Fase 8 manda mudar o `Status` da spec para `arquivada` no encerramento.
- Critério: **Dado** a Fase 8, **Quando** lida, **Então** o passo existe.

**R9** A degradação graciosa continua valendo. Em repositório sem o modelo do scaffold, as
convenções R1 a R8 valem como default da spec que a própria skill redige. A skill não ganha
dependência de script, plugin ou harness.
- Critério: **Dado** o `SKILL.md`, **Quando** se procura o nome de um script do scaffold como
  obrigatório, **Então** ele só aparece como "se existir".

**R10** O README, os exemplos de Tier 1 e Tier 2 e o diagrama Mermaid ficam coerentes com R1 a
R8. O `CHANGELOG.md` ganha a entrada `[3.0.0]`, e a tag `v3.0.0` é criada depois do merge.
- Critério: **Dado** a branch pronta, **Quando** se confere o "prove-it" deste repositório
  (Mermaid renderiza, links resolvem, exemplos consistentes), **Então** tudo passa.

## Não-objetivos

- **Fica para a 3.1.0, que acompanha o scaffold 1.8.0:**
  - Fase 4b (conferência de consistência);
  - busca de specs anteriores na Fase 0;
  - adendo de hotfix (M5).
- **Spec viva:** fora. O delta continua arquivado (ADR 0003 do portal).
- **Versão no front matter do `SKILL.md`:** fora. Campo extra pode quebrar harness que valida o
  front matter.

## Design proposto

- **`SKILL.md`:** edição das Fases 1, 3, 4, 6, 7 e 8 e dos critérios de saída. Um item novo em
  "Erros comuns": aprovar só na conversa. Sem fase nova, e a matriz tier → fases não muda.
- **`README.md` e `docs/exemplos/`:** os walkthroughs mostram R<n>, o cabeçalho de aprovação e
  a tabela de veredito.
- **Impacto Arquitetural:** N/A, porque é um repositório de documentação.

## Riscos e mitigações

- **O Tier 1 fica mais pesado.** R5 e R6 só pesam quando há spec, e Tier 1 não tem spec. O plano
  leve só cita o teste.
- **O `SKILL.md` cresce.** Meta: no máximo 40 linhas a mais; a revisão confere.

## Esclarecimentos

### Sessão 2026-10-05

- P: 2.1.0 ou 3.0.0? → R: 3.0.0, pela regra de versão deste `AGENTS.md`.
- P: E o PRD? → R: a Fase 1 o lê quando ele existe; a constituição do scaffold não muda.

## Threat-model

Não se aplica: documentação de processo, sem superfície de execução.
