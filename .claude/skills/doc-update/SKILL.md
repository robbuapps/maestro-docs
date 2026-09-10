---
name: doc-update
version: 1.0.0
description: >
  Recebe o relatório de uma análise de documentação (tipicamente a saída da skill `doc-review`,
  mas aceita também uma lista de achados passada diretamente) e propõe as edições correspondentes
  nas páginas do maestro-docs, seguindo o STYLE-GUIDE.md. Use quando o usuário pedir
  "/doc-update", "aplica esses achados na doc", "atualiza a documentação com base nisso", ou
  colar/referenciar um relatório do doc-review. Nunca commita ou dá push sozinha.
project: maestro-docs
organization: robbu
---

# Doc Update

Transforma um relatório de achados (de `doc-review`, ou fornecido diretamente pelo usuário) em
edições reais nas páginas do `maestro-docs`.

## Passo 1 — Carregar os padrões

Leia `STYLE-GUIDE.md` e `CONTENT-MAP.md` deste repositório antes de escrever qualquer coisa.
Eles não são opcionais — toda edição desta skill segue o que está lá.

## Passo 2 — Reverificar cada achado

Não confie cegamente no relatório recebido — ele pode ter sido gerado antes de outras mudanças
no código. Para cada página candidata:

1. Releia a fonte (diff da PR, `ai-context/`, ou o código apontado) para confirmar que a
   divergência ainda existe.
2. Se o achado já não procede (foi corrigido por outra pessoa, ou a leitura original estava
   errada), descarte e diga isso no resumo final — não edite a página.
3. Se o achado era classificado como **hipótese** e você não consegue elevá-lo a confirmado,
   pare e pergunte ao usuário antes de escrever qualquer coisa nessa página — não adivinhe o
   texto de algo incerto (mesma regra do `STYLE-GUIDE.md`: prefira remover/perguntar a arriscar).

## Passo 3 — Redigir a edição

Para cada achado confirmado, escreva a edição mínima que resolve a divergência — sem
aproveitar para reescrever o resto da página, a menos que o usuário peça. Siga em particular:

- Terminologia canônica do `STYLE-GUIDE.md` (não introduza um termo novo sem sinalizar).
- Nunca traduza/renomeie identificadores literais do sistema.
- Reaproveite os componentes já usados na página (`<Note>`, `<Warning>`, `<AccordionGroup>`,
  tabelas) em vez de inventar um formato novo.
- Se a mudança tocar um conceito documentado em mais de uma página (veja `CONTENT-MAP.md` e as
  "páginas transversais"), edite todas as ocorrências relevantes — não só a página apontada pelo
  relatório.

## Passo 4 — Página nova ou removida

Se o achado implica criar, renomear ou remover uma página:
- Atualize `docs.json` no mesmo conjunto de mudanças.
- Atualize `CONTENT-MAP.md` (linha do domínio/módulo correspondente).

## Passo 5 — Validar

Rode `npx mintlify@latest broken-links` depois de todas as edições. Corrija qualquer link
quebrado antes de seguir para o próximo passo.

## Passo 6 — Apresentar para confirmação

Nunca commite ou dê push sem aprovação explícita. Mostre um resumo:

```
## Atualizações propostas

- <página>: <resumo de uma linha da mudança> — achado: confirmado | reverificado e confirmado
- <página>: descartado — <motivo>
- <página>: pendente de confirmação — <o que precisa que o usuário decida>

Confirma o commit dessas mudanças?
```

Depois de aprovado, siga o fluxo de commit que o usuário indicar na sessão (branch dedicada vs.
commit direto) — não assuma qual vale por padrão; isso é uma decisão por sessão, não uma regra
fixa desta skill.

## Referência

- Padrões: `STYLE-GUIDE.md`
- Mapa de conteúdo: `CONTENT-MAP.md`
- Validação: `npx mintlify@latest broken-links`
