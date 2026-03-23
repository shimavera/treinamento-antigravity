# MCP — Evolution API

Conecta o Claude Code a sua instancia da Evolution API (WhatsApp).
Com esse MCP, o Claude consegue verificar instancias, enviar mensagens, configurar webhooks — direto pelo chat.

## O que ele faz

- Lista instancias WhatsApp
- Verifica status de conexao
- Envia mensagens de texto e midia
- Configura webhooks
- Gera QR Code para conectar
- Gerencia grupos

## Pre-requisito

Voce precisa ter uma Evolution API rodando. Pode ser:
- **Self-hosted** em uma VPS (Docker)
- **Cloud** em um servico gerenciado

### Instalar Evolution API com Docker

```bash
docker run -d \
  --name evolution-api \
  -p 8080:8080 \
  -e AUTHENTICATION_API_KEY=sua-chave-aqui \
  atendai/evolution-api:latest
```

## Como configurar o MCP

### 1. Instale o servidor MCP da Evolution API

```bash
cd ~/.claude/mcp-servers
git clone https://github.com/shimavera/evolution-api-mcp.git evolution-api
cd evolution-api
npm install && npm run build
```

> Ou use o MCP que esta neste repositorio se disponivel.

### 2. Adicione ao Claude Code

Edite `~/.claude/settings.json` e adicione dentro de `"mcpServers"`:

```json
{
  "mcpServers": {
    "evolution-api": {
      "command": "node",
      "args": [
        "/Users/SEU_USUARIO/.claude/mcp-servers/evolution-api/build/index.js"
      ],
      "env": {
        "EVOLUTION_API_URL": "https://SUA_URL_EVOLUTION",
        "EVOLUTION_API_KEY": "SUA_API_KEY"
      }
    }
  }
}
```

Substitua:
- `SEU_USUARIO` pelo seu usuario do sistema
- `SUA_URL_EVOLUTION` pela URL da sua Evolution API
- `SUA_API_KEY` pela chave de API

### 3. Reinicie o Claude Code

### 4. Teste

```
Liste minhas instancias do WhatsApp
```

## Dicas

- A Evolution API precisa estar rodando (VPS ou local) para o MCP funcionar
- Conecte uma instancia via QR Code antes de usar
- O MCP usa a API REST da Evolution por baixo
