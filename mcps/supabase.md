# MCP — Supabase

Conecta o Claude Code diretamente ao seu banco de dados Supabase.
Com esse MCP, o Claude consegue listar tabelas, rodar queries, ver dados reais — sem voce abrir o painel.

## O que ele faz

- Lista todas as tabelas do banco
- Roda queries SQL
- Cria tabelas e migrations
- Verifica dados reais
- Consulta schema completo

## Como configurar

### 1. Pegue suas credenciais no Supabase

1. Acesse [supabase.com](https://supabase.com) e abra seu projeto
2. Va em **Settings > API**
3. Copie:
   - **Project URL** (ex: `https://xxxxxxxxxxxx.supabase.co`)
   - **Service Role Key** (a chave `service_role`, NAO a `anon`)

### 2. Adicione ao Claude Code

Edite o arquivo `~/.claude/settings.json` e adicione dentro de `"mcpServers"`:

```json
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": [
        "-y",
        "@supabase/mcp-server-supabase@latest",
        "--supabase-url",
        "SUA_PROJECT_URL",
        "--service-role-key",
        "SUA_SERVICE_ROLE_KEY"
      ]
    }
  }
}
```

### 3. Reinicie o Claude Code

Feche e abra novamente. O MCP vai conectar automaticamente.

### 4. Teste

Pergunte ao Claude:
```
Liste todas as tabelas do meu banco de dados
```

Se funcionar, voce vera a lista de tabelas do seu projeto Supabase.

## Dicas

- Use a chave `service_role` (tem acesso total), NAO a `anon`
- Cada projeto Supabase precisa de um MCP separado
- O MCP roda localmente — suas credenciais nao saem do seu computador
