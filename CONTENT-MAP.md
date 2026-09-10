# Mapa de Conteúdo

Relaciona o que existe no código do Maestro (domínios de negócio do backend e módulos/rotas do frontend) às páginas deste site que dependem deles. O objetivo é dar um ponto de partida rápido para responder "este PR/card pode ter deixado alguma página desatualizada?" sem precisar conhecer a estrutura do `maestro-docs` de cor.

Cobre dois repositórios distintos:

- **`Robbu.Maestro.API`** (backend) — domínios documentados em `ai-context/*-behavior.md`.
- **`Robbu.Maestro.API/src/frontend/robbu-maestro-app`** (frontend, repositório próprio) — módulos documentados em `ai-context/01-repo-map.md`. Esse repo não tem `ai-context` de regra de negócio, só de estrutura/convenção técnica — a fonte de verdade de comportamento continua sendo o backend.

## Como é consumido

Este mapa **não** é checado durante a revisão de código (decisão deliberada — um PR isolado raramente mostra o quadro completo de uma mudança de comportamento). Ele é consumido pela skill `doc-review`, que analisa um work item do ADO inteiro (card + subtasks + todas as PRs vinculadas, em qualquer um dos dois repositórios) e usa este mapa para levantar páginas candidatas a atualização. O relatório resultante alimenta a skill `doc-update`, que propõe as edições seguindo `STYLE-GUIDE.md`.

## Backend: Domínio → Página

| Domínio (`ai-context/`) | Arquivo fonte | Páginas afetadas |
|---|---|---|
| Brokers | `80-brokers-behavior.md` | `integracoes/integracao-com-provedores.mdx`, `guia-do-usuario/publico.mdx` (coluna `BROKER`), `integracoes/uso-de-sftp.mdx`, `integracoes/direct-message-api.mdx` (campo `broker`) |
| Campaigns | `81-campaigns-behavior.md` | `guia-do-usuario/campanhas.mdx`, `guia-do-usuario/publico.mdx`, `integracoes/uso-de-sftp.mdx` (importar campanhas) |
| Companies | `82-companies-behavior.md` | `guia-do-usuario/empresas-e-usuarios.mdx`, `introducao/perfis-de-uso.mdx` |
| Dashboards | `83-dashboards-behavior.md` | `guia-do-usuario/relatorios.mdx` (quando houver conteúdo) |
| MessageTemplates | `84-message-templates-behavior.md` | `guia-do-usuario/templates.mdx`, `central-de-suporte/codigos-de-erro-e-status.mdx` (status de template) |
| Messages | `85-messages-behavior.md` | `integracoes/webhook-de-eventos.mdx`, `central-de-suporte/codigos-de-erro-e-status.mdx` (status de mensagem), `integracoes/direct-message-api.mdx` |
| Reports | `86-reports-behavior.md` | `guia-do-usuario/relatorios.mdx` |
| Rules | `87-rules-behavior.md` | `motor-de-regras/visao-geral.mdx`, `central-de-suporte/troubleshooting.mdx` |
| Segments | `88-segments-behavior.md` | `guia-do-usuario/segmentos.mdx`, `integracoes/integracao-com-provedores.mdx` (direcionamento por segmento) |
| Tenants | `89-tenants-behavior.md` | `introducao/perfis-de-uso.mdx`, `guia-do-usuario/configuracoes.mdx` |
| Users | `90-users-behavior.md` | `guia-do-usuario/empresas-e-usuarios.mdx` |
| Permissions | `91-permissions-behavior.md` | `guia-do-usuario/empresas-e-usuarios.mdx` (perfis de usuário) |
| Import layout | `93-import-layout-detection.md` | `integracoes/mapeamento-de-layouts.mdx`, `integracoes/uso-de-sftp.mdx`, `guia-do-usuario/publico.mdx`, `motor-de-regras/visao-geral.mdx` (Contatos Autorizados/Bloqueados) |

## Frontend: Módulo → Página

| Módulo (`src/modules/`) | Rota(s) | Páginas afetadas |
|---|---|---|
| `auth` | `/auth/*` | `introducao/visao-geral.mdx` (login) |
| `campaigns` | `/campaigns`, `/campaigns/:id`, `/campaigns/criar` | `guia-do-usuario/campanhas.mdx` |
| `company` | `/empresas`, `/empresas/:id`, `/empresas/criar` | `guia-do-usuario/empresas-e-usuarios.mdx` |
| `configuration` | `/configuracoes/*` | `guia-do-usuario/configuracoes.mdx`, `motor-de-regras/visao-geral.mdx`, `integracoes/integracao-com-provedores.mdx`, `integracoes/uso-de-sftp.mdx`, `integracoes/webhook-de-eventos.mdx` |
| `dashboard` | `/dashboards`, `/dashboards/campanhas`, `/dashboards/templates`, `/dashboards/canais` | `guia-do-usuario/relatorios.mdx` (quando houver conteúdo) |
| `help` | `/ajuda` | `central-de-suporte/faq.mdx`, `central-de-suporte/troubleshooting.mdx` |
| `import-layout-mappings` | `/configuracoes/mapeamento-colunas`, `/criar`, `/:id/editar` | `integracoes/mapeamento-de-layouts.mdx` |
| `mailing` | `/publico` | `guia-do-usuario/publico.mdx` |
| `permissions` | `/configuracoes/ambiente/features`, `/configuracoes/empresa/perfis-de-acesso` | `guia-do-usuario/empresas-e-usuarios.mdx` (perfis de usuário) |
| `reports` | `/relatorios` | `guia-do-usuario/relatorios.mdx` |
| `segments` | `/segmentos` | `guia-do-usuario/segmentos.mdx` |
| `templates` | `/templates/*` | `guia-do-usuario/templates.mdx` |
| `templates-review` | `/templates-review` | `guia-do-usuario/templates.mdx` (seção Revisão de Templates) |
| `tenant` | — | `introducao/perfis-de-uso.mdx` |
| `user` | `/usuarios`, `/usuarios/:id`, `/usuarios/criar`, `/usuarios/perfil` | `guia-do-usuario/empresas-e-usuarios.mdx` |
| `waba` | — | `integracoes/integracao-com-provedores.mdx` (WhatsApp) |

`app`, `home` e `websocket` não têm página correspondente hoje (infraestrutura interna / tela sem documentação dedicada).

## Páginas transversais

Estas páginas podem ser afetadas por qualquer domínio, então o mapa acima não as cobre bem — um PR que mexe em qualquer feature pode, em tese, exigir revisão delas. Não são bons alvos para um gatilho por PR; ficam melhor cobertas por uma auditoria periódica.

- `central-de-suporte/troubleshooting.mdx`
- `central-de-suporte/codigos-de-erro-e-status.mdx`
- `central-de-suporte/faq.mdx`
- `introducao/glossario.mdx`

## Manutenção

Este mapa é mantido por quem edita o `maestro-docs`. Ao renomear, criar ou remover uma página, atualize a linha correspondente aqui no mesmo commit/PR — mesma lógica do `ai-context/` no `Robbu.Maestro.API`: documentação viva, refletindo o estado atual, sem histórico de mudanças (isso o Git já guarda).
