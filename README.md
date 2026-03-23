# Treinamento IA Antigravity + Claude Code

Skills e MCPs para usar no Claude Code / Antigravity durante o treinamento.

---

## Skills disponiveis (9)

### Automacao & WhatsApp
| Skill | O que faz |
|-------|-----------|
| `evolution-api` | Configurar WhatsApp, webhooks, instancias |
| `n8n-workflow-patterns` | Padroes de automacao no n8n |
| `n8n-code-javascript` | Escrever codigo dentro do n8n |
| `n8n-expression-syntax` | Expressoes e variaveis do n8n |
| `n8n-node-configuration` | Configurar nos corretamente |

### Obsidian
| Skill | O que faz |
|-------|-----------|
| `obsidian-cli` | Interagir com o vault (criar, buscar, gerenciar notas) |
| `obsidian-markdown` | Criar notas com wikilinks, callouts, embeds |
| `obsidian-bases` | Criar views, filtros e formulas no Obsidian |

### Ferramentas
| Skill | O que faz |
|-------|-----------|
| `mcp-builder` | Criar servidores MCP para conectar o Claude a APIs |

### Como instalar skills

**Opcao 1 — Clonar tudo (mais facil)**

```bash
git clone https://github.com/shimavera/treinamento-antigravity.git
cp -r treinamento-antigravity/skills/* ~/.claude/skills/
```

**Opcao 2 — Instalar uma por uma no Claude Code**

```
/install-skill https://raw.githubusercontent.com/shimavera/treinamento-antigravity/main/skills/evolution-api/SKILL.md
/install-skill https://raw.githubusercontent.com/shimavera/treinamento-antigravity/main/skills/n8n-workflow-patterns/SKILL.md
/install-skill https://raw.githubusercontent.com/shimavera/treinamento-antigravity/main/skills/n8n-code-javascript/SKILL.md
/install-skill https://raw.githubusercontent.com/shimavera/treinamento-antigravity/main/skills/n8n-expression-syntax/SKILL.md
/install-skill https://raw.githubusercontent.com/shimavera/treinamento-antigravity/main/skills/n8n-node-configuration/SKILL.md
/install-skill https://raw.githubusercontent.com/shimavera/treinamento-antigravity/main/skills/obsidian-cli/SKILL.md
/install-skill https://raw.githubusercontent.com/shimavera/treinamento-antigravity/main/skills/obsidian-markdown/SKILL.md
/install-skill https://raw.githubusercontent.com/shimavera/treinamento-antigravity/main/skills/obsidian-bases/SKILL.md
/install-skill https://raw.githubusercontent.com/shimavera/treinamento-antigravity/main/skills/mcp-builder/SKILL.md
```

---

## MCPs — Conectar o Claude a servicos reais

MCPs permitem que o Claude acesse bancos de dados, WhatsApp e automacoes direto pelo chat.
Cada guia explica como configurar passo a passo:

| MCP | Guia | O que faz |
|-----|------|-----------|
| Supabase | [mcps/supabase.md](mcps/supabase.md) | Consultar banco de dados, rodar SQL, criar tabelas |
| Evolution API | [mcps/evolution-api.md](mcps/evolution-api.md) | Gerenciar WhatsApp, enviar mensagens, webhooks |
| n8n | [mcps/n8n.md](mcps/n8n.md) | Listar workflows, verificar execucoes, buscar erros |

---

## Projeto do treinamento

Resumo automatico de grupos WhatsApp usando:
- **Evolution API** — receber mensagens do WhatsApp
- **n8n** — automacoes visuais
- **Supabase** — banco de dados
- **Claude API** — gerar resumos com IA
