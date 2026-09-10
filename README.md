# Maestro Docs

Documentação pública do Maestro (Robbu), construída com [Mintlify](https://mintlify.com).

## Rodando localmente

```bash
npm i -g mintlify
mintlify dev
```

## Estrutura

| Pasta | Conteúdo |
|---|---|
| `introducao/` | Visão geral (inclui login), perfis de uso, glossário |
| `guia-do-usuario/` | Guias de uso por funcionalidade (público, campanhas, templates, segmentos, relatórios, empresas/usuários, configurações) |
| `motor-de-regras/` | Regras de restrição e bloqueio de envios |
| `integracoes/` | Direct Message API, Webhook, Provedores, SFTP |
| `central-de-suporte/` | FAQ, troubleshooting e referência de códigos de erro/status — conteúdo novo, sem equivalente em `docs.robbu.global` |

A navegação completa está em [`docs.json`](docs.json).

## Origem do conteúdo

Este site é dedicado exclusivamente ao Maestro e substitui a seção `/docs/maestro` de `docs.robbu.global`. As páginas ainda em construção citam, no aviso do topo, de qual página de origem o conteúdo será migrado, e/ou qual documento de `ai-context/` do repositório `Robbu.Maestro.API` deve ser usado para validar regras de negócio antes de publicar.

## Convenções

Ver [`STYLE-GUIDE.md`](STYLE-GUIDE.md) para padrões de escrita, terminologia e componentes.

- Um arquivo `.mdx` por página, em `kebab-case` sem acentos.
- Toda página nova entra no `docs.json` dentro do grupo correspondente.
- Páginas de funcionalidade devem linkar para `central-de-suporte/troubleshooting` quando houver erros/edge-cases conhecidos.

## Mantendo a documentação atualizada

[`CONTENT-MAP.md`](CONTENT-MAP.md) relaciona cada domínio de negócio do Maestro às páginas que dependem dele. Ao renomear, criar ou remover uma página, atualize esse mapa no mesmo commit.

Duas skills usam esse mapa para manter a doc em dia, no nível do work item (não da revisão de código):

- **`doc-review <card>`** — analisa um card do ADO (card + subtasks + PRs vinculadas, nos dois repositórios do Maestro) e levanta páginas candidatas a atualização. Só investiga e relata, não edita.
- **`doc-update`** — recebe o relatório do `doc-review` e propõe as edições correspondentes, seguindo o `STYLE-GUIDE.md`. Nunca commita/dá push sem confirmação.
