---
name: doc-review
version: 1.0.0
description: >
  Analisa um work item do Azure DevOps (card, suas subtasks e as PRs vinculadas a eles — no
  Robbu.Maestro.API e/ou no robbu-maestro-app) para identificar páginas do maestro-docs que
  podem precisar de atualização. Use quando o usuário pedir "/doc-review", "revisa a
  documentação do card X", "esse work item mudou algo que afeta a doc?", ou similar, sempre
  passando um número de card do ADO. Não edita nenhuma página — só investiga e relata. A saída
  é o input direto da skill `doc-update`.
project: maestro-docs
organization: robbu
---

# Doc Review

Investiga se um work item concluído (ou em andamento) deixou alguma página do `maestro-docs`
desatualizada. Roda no nível do work item — não de uma PR isolada — porque uma mudança de
comportamento raramente cabe inteira numa única PR; card, subtasks e todas as PRs vinculadas
juntas é que mostram o quadro completo.

**Nunca edita arquivos.** Produz um relatório estruturado que a skill `doc-update` consome.

## Passo 1 — Resolver o work item

Peça o número do card se não foi informado. Chame `wit_get_work_item` (projeto `Engenharia`,
`expand: relations`) para obter título, descrição, tipo e estado.

## Passo 2 — Enumerar subtasks

Nas relations do work item, siga os links `System.LinkTypes.Hierarchy-Forward` (filhos). Para
cada subtask encontrada, repita a resolução do Passo 1.

Junte a descrição do card pai + todas as subtasks num único contexto de "o que este trabalho
deveria ter mudado" — isso ajuda a julgar se uma mudança de código é ruído ou é o que de fato
foi entregue.

## Passo 3 — Enumerar PRs vinculadas

Nas relations do card e de cada subtask, identifique links de Pull Request (`ArtifactLink` para
`vstfs:///Git/PullRequestId/...`). Uma PR pode estar tanto no repositório `Robbu.Maestro.API`
quanto no `robbu-maestro-app` (frontend) — não assuma um só repositório.

Se nenhuma PR for encontrada (work item ainda não tem código), pare aqui e informe:
```
ℹ Card #<id> não tem nenhuma PR vinculada ainda. Nada para analisar.
```

## Passo 4 — Levantar arquivos alterados

Para cada PR, obtenha a lista de arquivos alterados (`mcp__ado__repo_pull_request` ou
equivalente). Agrupe por repositório.

## Passo 5 — Cruzar com o mapa de conteúdo

Busque `CONTENT-MAP.md` deste repositório (já está local, é só ler). Cruze os arquivos/projetos
alterados com as duas tabelas ("Backend: Domínio → Página" e "Frontend: Módulo → Página").

## Passo 6 — Verificar de verdade, não só cruzar caminho

Cruzar caminho de arquivo com o mapa dá **candidatos**, não confirmação. Para cada candidato:

1. Leia o diff da(s) PR(s) relevante(s) — não só os nomes de arquivo, o conteúdo.
2. Leia o conteúdo atual da página do `maestro-docs` apontada.
3. Julgue se há divergência real entre o que a página diz e o que o código agora faz.
4. Se o diff sozinho não for conclusivo (ex.: mudança de nome de status, remoção de um
   comportamento, um conceito que a doc descreve mas você não encontra no código), investigue
   mais — grep no repositório, leitura de `ai-context/`, até chegar numa resposta com evidência.
   Não deixe isso para a skill `doc-update`; ela vai reverificar, mas o relatório precisa ser
   útil por si só.

Classifique cada candidato como o `STYLE-GUIDE.md` pede: **confirmado no código**, **inferido
com evidência**, ou **hipótese** — e diga qual das três é, explicitamente, no relatório.

## Passo 7 — Relatório final

```
# Doc Review — Card #<id> <título>

## Resumo do que foi entregue
[2-4 frases combinando card + subtasks + PRs]

## Páginas candidatas

### <caminho/da/pagina.mdx>
**Classificação:** confirmado no código | inferido com evidência | hipótese
**Motivo:** <PR #N / subtask #M — o que mudou>
**O que parece estar desatualizado:** <descrição concreta, citando o texto atual da página
quando fizer sentido>

[repetir por página candidata]

## Sem candidatos
[se nenhuma página foi flagrada, diga isso claramente e pare aqui]
```

Se um candidato ficar em **hipótese** e você não conseguir elevar a confiança investigando mais,
diga isso explicitamente no relatório em vez de omitir — a skill `doc-update` (ou um humano) decide
se vale seguir.
