# Guia de Estilo

Padrões de escrita e organização fixados durante a construção deste site. Serve de referência tanto para quem escreve conteúdo manualmente quanto para as skills `doc-review`/`doc-update`.

## Prosa

- Escreva a versão mais enxuta do fato, não a frase de quem explicou o fato. Ao incorporar uma explicação de alguém (usuário, PR, work item), reescreva — não transcreva.
- Prefira frases curtas e diretas a período composto.
- Não repita "isso fica em outra tela, não aqui" dos dois lados de uma referência — diga o fato uma vez e deixe o link carregar o "onde fica".
- **Nunca descreva um comportamento por contraste com um estado anterior que o leitor não tem como conhecer** (ex.: "segue como funcionava antes dessa funcionalidade existir", "agora passou a exigir X"). O leitor de uma doc de produto não tem o "antes" como referência — só o que existe hoje. Descreva o comportamento atual de forma completa e autossuficiente, mesmo quando a informação chegou até você como um diff ou uma mudança ("card #X mudou Y para Z"). Isso vale mesmo que o texto de origem (PR, work item, explicação do usuário) esteja fraseado como mudança — a skill `doc-review` pode (e deve) registrar o achado como um delta; a skill `doc-update`, ao escrever a página, converte esse delta no fato presente, nunca na comparação.
- **A mesma regra vale quando dois sistemas coexistem durante uma migração em fases** (ex.: um modelo de permissão novo rodando ao lado do antigo, enquanto o antigo ainda não foi desligado). Não ancore a explicação do sistema novo no antigo — nada de "no lugar do perfil fixo", "diferente de como funcionava antes". Escreva o sistema novo como se fosse a única explicação que existe, mesmo cobrindo hoje só parte do produto. Se for preciso indicar até onde ele já chega, diga isso como um fato próprio (ex.: uma lista do que já usa o modelo novo, com uma nota de que o resto chega em breve) — nunca como comparação com o mecanismo antigo. A ressalva de escopo do sistema antigo (o que ele ainda cobre) mora na própria página dele, não na página do novo.

## Links entre páginas

- Link para a página, não para uma âncora de seção (`#secao`), a menos que o título da seção seja puro ASCII sem acento — o slug que o Mintlify gera para títulos acentuados não é previsível o suficiente para confiar.
- Prefira `[Termo](/caminho/da-pagina)` a repetir a explicação já escrita em outra página.

## Terminologia

- Mantenha um termo canônico por conceito em toda a prosa (ex.: neste site, "envio"/"enviar" — não "disparo"/"disparar"; "provedor" — não "broker"). Ao trocar um termo, faça a varredura completa do site, não só o trecho pedido — mas **pergunte antes de aplicar a outras ocorrências** que pareçam similares e não tenham sido explicitamente confirmadas.
- **Nunca traduza ou renomeie identificadores literais** do sistema: nomes de coluna de CSV (`broker`, `broker_account_reference`, `dispatch_at`), campos de JSON de API (`"broker"`), nomes de header HTTP, códigos de erro. Esses são contratos reais, não prosa. Isso vale mesmo quando o próprio sistema renomeia esses identificadores (ex.: o padrão de colunas de importação passou de português maiúsculo para inglês minúsculo em 2026-09) — reflita o nome atual exatamente como está no código, não invente uma tradução própria.

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
