# Guia de Estilo

Padrões de escrita e organização fixados durante a construção deste site. Serve de referência tanto para quem escreve conteúdo manualmente quanto para as skills `doc-review`/`doc-update`.

## Prosa

- Escreva a versão mais enxuta do fato, não a frase de quem explicou o fato. Ao incorporar uma explicação de alguém (usuário, PR, work item), reescreva — não transcreva.
- Prefira frases curtas e diretas a período composto.
- Não repita "isso fica em outra tela, não aqui" dos dois lados de uma referência — diga o fato uma vez e deixe o link carregar o "onde fica".

## Links entre páginas

- Link para a página, não para uma âncora de seção (`#secao`), a menos que o título da seção seja puro ASCII sem acento — o slug que o Mintlify gera para títulos acentuados não é previsível o suficiente para confiar.
- Prefira `[Termo](/caminho/da-pagina)` a repetir a explicação já escrita em outra página.

## Terminologia

- Mantenha um termo canônico por conceito em toda a prosa (ex.: neste site, "envio"/"enviar" — não "disparo"/"disparar"; "provedor" — não "broker"). Ao trocar um termo, faça a varredura completa do site, não só o trecho pedido — mas **pergunte antes de aplicar a outras ocorrências** que pareçam similares e não tenham sido explicitamente confirmadas.
- **Nunca traduza ou renomeie identificadores literais** do sistema: nomes de coluna de CSV (`BROKER`, `BROKER_REFERENCE`, `DATA_HORA_DISPARO`), campos de JSON de API (`"broker"`), nomes de header HTTP, códigos de erro. Esses são contratos reais, não prosa.

## Precisão

- Nunca afirme um comportamento que não foi confirmado no código, no `ai-context/` ou por alguém do time. Quando uma alegação não puder ser confirmada, não a inclua "por garantia" — prefira remover a afirmação a arriscar publicar algo errado (caso real: o status "Em reaprovação" não existe em lugar nenhum do código e foi removido, não mantido com uma ressalva).
- Quando algo for confirmado mas com uma lacuna conhecida (ex.: um valor de enum não documentado), sinalize com `<Warning>` explicando a lacuna, em vez de inventar o valor.
- Classifique mentalmente cada afirmação: confirmado no código/ai-context, inferido com evidência, ou hipótese — só a primeira categoria vai para a página sem ressalva.

## Componentes Mintlify

- `<Note>` — contexto neutro, avisos de "página em construção", relações entre conceitos.
- `<Warning>` — pegadinhas, ações irreversíveis, lacunas de informação conhecidas.
- `<AccordionGroup>` / `<Accordion>` — FAQs.
- Bloco de código com `title="..."` para exemplos de request/response nomeados (ex.: ` ```json title="409 - Conflict" `).
- Tabelas para referência estruturada (campos de arquivo, códigos de status, permissões).

## Armadilhas de MDX

- `{{Var1}}` (variáveis de template) só aparece dentro de crase ou bloco de código — nunca solto em prosa, ou o MDX tenta interpretar como expressão JS.
- `|` literal dentro de célula de tabela precisa ser escapado como `` `\|` ``.
- Depois de qualquer edição, rode `npx mintlify@latest broken-links` antes de considerar a mudança pronta.

## Manutenção estrutural

- Página nova, renomeada ou removida → atualize `docs.json` e `CONTENT-MAP.md` no mesmo commit.
- Nunca commite ou dê push sem confirmação explícita do usuário.
