---
name: doc-card
version: 1.0.0
description: >
  Orquestra o ciclo completo de revisão de documentação para um work item do Azure DevOps:
  roda `doc-review`, alimenta o relatório resultante em `doc-update`, e termina sempre no mesmo
  checkpoint — apresenta as mudanças propostas, pergunta se falta ajuste, e oferece criar uma
  branch nova a partir da `main` atualizada do `maestro-docs` e abrir a PR no GitHub. Use quando
  o usuário pedir "/doc-card X", "revisa e atualiza a doc do card X", "faz o doc-review e
  doc-update do card X", ou pedir para cuidar de ponta a ponta da documentação de um work item,
  sempre passando um número de card do ADO. Não substitui `doc-review`/`doc-update` standalone —
  é o atalho para o caso comum em que as duas rodam em sequência mesmo.
project: maestro-docs
organization: robbu
---

# Doc Card

Atalho de ponta a ponta para o par `doc-review` + `doc-update`: investiga o work item, propõe as
edições, e — só depois de confirmação explícita — cria a branch, comita, dá push e abre a PR.

Existe porque, na prática, `doc-review` quase sempre é seguido de `doc-update` no mesmo card;
as duas skills continuam valendo standalone para quem quiser só o relatório (sem editar) ou
quiser aplicar uma lista de achados curada manualmente.

## Passo 1 — Rodar a revisão

Peça o número do card se não foi informado. Invoque a skill `doc-review` com esse número
(`Skill(doc-review, args: "<card>")`) e obtenha o relatório completo.

Se o relatório voltar em **Sem candidatos** (nenhuma página flagrada), pare aqui e informe o
resultado — não prossiga para o Passo 2.

## Passo 2 — Aplicar as edições

Invoque a skill `doc-update` (`Skill(doc-update, args: "<relatório do passo 1>")`) para
reverificar cada achado e escrever as edições. Siga as regras dela integralmente (`STYLE-GUIDE.md`,
`CONTENT-MAP.md`, validação de links) — a única parte que este skill substitui é o checkpoint
final dela (Passo 6 de `doc-update`): não pergunte lá sobre fluxo de commit, isso é resolvido no
Passo 3 abaixo.

Se `doc-update` ficar bloqueado numa **hipótese** não confirmável, pare e pergunte ao usuário
como o próprio `doc-update` manda — não avance para o Passo 3 com uma página pendente.

## Passo 3 — Checkpoint único

Depois das edições e da validação (`npx mintlify@latest broken-links` sem erros), apresente
sempre o mesmo formato, independente de quantas páginas foram tocadas:

```
## Atualizações propostas

- <página>: <resumo de uma linha da mudança> — achado: confirmado | reverificado e confirmado
- <página>: descartado — <motivo>
- <página>: pendente de confirmação — <o que precisa que o usuário decida>

Confirma essas mudanças? Posso criar uma branch nova a partir da main atualizada do
maestro-docs e abrir a PR?
```

Não crie branch, não comite e não dê push antes dessa confirmação explícita.

## Passo 4 — Branch, commit e PR

Só depois do "sim":

1. **Base limpa**: rode `git status`. Se houver mudanças não relacionadas já em andamento na
   working tree, avise o usuário em vez de descartá-las ou empacotá-las junto.
2. **Atualize a `main` local**: `git checkout main && git pull origin main`.
3. **Branch nova a partir da `main` atualizada**: `git checkout -b doc-review-card-<numero>`
   (não a partir da branch atual — o objetivo é a PR não carregar commits de outro trabalho em
   andamento). Se as edições do Passo 2 já tiverem sido feitas em cima de outra branch, mova-as
   para a branch nova (`git stash` na branch antiga, `git stash pop` na nova) em vez de refazer
   o diff.
4. **Commit**: mensagem referenciando o card e, quando fizer sentido, as PRs de origem
   (ex.: `Doc-review pass: <resumo> (card #<numero>)`).
5. **Push**: `git push -u origin doc-review-card-<numero>`.
6. **Abrir a PR** contra `main` no repositório GitHub do `maestro-docs`. Tente `gh pr create`
   primeiro; se `gh` não estiver disponível no ambiente, use
   `mcp__github__create_pull_request` (owner/repo do remote do projeto). Corpo da PR:
   resumo do que foi documentado, link/referência do card e das PRs de origem (ADO), e um item
   de test plan confirmando que `broken-links` passou.
7. Devolva a URL da PR ao usuário.

## Referência

- Skills: `doc-review`, `doc-update`
- Padrões: `STYLE-GUIDE.md`, `CONTENT-MAP.md`
- Validação: `npx mintlify@latest broken-links`
