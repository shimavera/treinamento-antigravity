# MCP — n8n

Conecta o Claude Code ao seu n8n.
Com esse MCP, o Claude consegue listar workflows, verificar execucoes, buscar erros — direto pelo chat.

## O que ele faz

- Lista todos os workflows
- Verifica execucoes recentes
- Busca nos com erro
- Ativa/desativa workflows
- Consulta configuracao de nos

## Pre-requisito

Voce precisa de um n8n rodando com API habilitada:
- **n8n Cloud** (trial gratis)
- **Self-hosted** em VPS (Docker)

### Gerar API Key no n8n

1. Abra seu n8n
2. Va em **Settings > API**
3. Clique em **Create API Key**
4. Copie a chave gerada

## Como configurar o MCP

Edite `~/.claude/settings.json` e adicione dentro de `"mcpServers"`:

```json
{
  "mcpServers": {
    "n8n": {
      "command": "npx",
      "args": [
        "-y",
        "n8n-mcp"
      ],
      "env": {
        "N8N_API_URL": "https://SUA_URL_N8N",
        "N8N_API_KEY": "SUA_API_KEY_N8N"
      }
    }
  }
}
```

Substitua:
- `SUA_URL_N8N` pela URL do seu n8n (ex: `https://meu-n8n.dominio.com`)
- `SUA_API_KEY_N8N` pela chave de API gerada

### Reinicie o Claude Code

### Teste

```
Liste meus workflows do n8n
```

## Dicas

- O pacote `n8n-mcp` e instalado automaticamente pelo `npx`
- Funciona com n8n Cloud e self-hosted
- A API Key tem acesso total — nao compartilhe
