# Roteiro — Treinamento IA Antigravity + Claude Code
**Data:** 23 de Março, 2026
**Formato:** Google Meet — tela compartilhada
**Duração estimada:** 2-3 horas

---

## Checklist Pré-Curso (fazer ANTES de iniciar)

### Instrutor
- [ ] Evolution API com número conectado e instância ativa
- [ ] Grupo WhatsApp de teste criado com ~10 mensagens
- [ ] Projeto Supabase com tabela `group_messages` criada (SQL abaixo)
- [ ] n8n aberto com workflows template prontos para importar
- [ ] API Key Claude com créditos (console.anthropic.com)
- [ ] Conta demo Abacate Pay ativa
- [ ] Apresentação `apresentacao.html` aberta no navegador
- [ ] Terminal aberto com Claude Code rodando
- [ ] Meet configurado com compartilhamento de tela
- [ ] Segunda tela (ou janelas organizadas): slides + terminal + navegador

### Enviar para alunos (1 dia antes)
- [ ] Link de criação de conta Supabase
- [ ] Link de criação de conta GitHub
- [ ] Link de criação de conta Vercel
- [ ] Instruções para instalar Node.js
- [ ] Link do Meet

---

## Estrutura do Curso

### BLOCO 1 — Apresentação (30 min)
**Tela:** Slides (apresentacao.html)

#### Slide 1-2: Abertura + Agenda
- Apresentar-se brevemente
- Mostrar agenda do dia
- "Hoje vocês vão sair daqui com um projeto real funcionando"

#### Slide 3-4: O que é Antigravity / Claude Code
- **Ponto-chave:** É uma IDE de IA no terminal
- Você conversa → ela implementa
- Mostrar que Antigravity = Claude Code por baixo
- **Demo rápida:** Abrir terminal, rodar `claude`, fazer uma pergunta simples

#### Slide 5-6: Skills
- **Analogia:** "Skills são manuais de instrução para o Claude"
- É como dar um livro de receitas para um chef — ele sabe cozinhar, mas com o livro faz pratos específicos muito melhor
- Mostrar lista de skills que usaremos
- **Como instalar:** `/install-skill URL` ou arquivo em `~/.claude/skills/`

#### Slide 7-8: MCPs
- **Analogia:** "MCPs são as mãos do Claude"
- Sem MCP: "abre o painel, copia a query, cola lá"
- Com MCP: "Claude, quantos registros tem?" → resposta direto
- Mostrar configuração no `settings.json`

#### Slide 9-10: Obsidian
- **Por que Obsidian?**
  - Segundo cérebro — notas em Markdown
  - O Claude LÊ suas notas = contexto permanente
  - Não precisa explicar tudo toda vez
  - Funciona como memória de longo prazo entre sessões
- **Estrutura do vault:**
  - Projetos, Infraestrutura, Conhecimento, Padrões, Templates
  - INDEX.md como mapa geral
- **Dica:** "Quanto melhor seu Obsidian, melhor o Claude trabalha pra você"

#### Slide 11: APIs, Webhooks, API Keys
- **API = garçom do restaurante** (pedido → cozinha → resposta)
- **API Key = crachá** (prova que você tem permissão)
- **Webhook = notificação automática** (algo acontece → você é avisado)
- Dar exemplo concreto: "mensagem no grupo → webhook → n8n"

---

### BLOCO 2 — Stack Gratuita (20 min)
**Tela:** Slides + Navegador (demos rápidas)

#### Slide 12: Stack completa
Para cada ferramenta, abrir no navegador e mostrar:

**Supabase (5 min)**
- Abrir supabase.com → mostrar dashboard
- "Banco de dados PostgreSQL na nuvem, grátis"
- Mostrar: tabelas, SQL editor, API settings
- Free tier: 500MB, 50K requests/mês

**GitHub (3 min)**
- "Onde guardamos o código"
- Repositórios ilimitados grátis
- "Git = controle de versão, GitHub = onde fica na nuvem"

**Vercel (3 min)**
- "Deploy automático"
- Push no GitHub → site atualizado em segundos
- Free tier com domínio .vercel.app

**Evolution API (3 min)**
- "Conecta ao WhatsApp via API"
- Open-source, Baileys engine
- Instâncias, QR Code, webhooks

**Abacate Pay (3 min)**
- "Pagamentos via Pix brasileiro"
- Webhook quando pagamento confirma
- Conta demo para testes

**n8n (3 min)**
- "Automações visuais — o Zapier open-source"
- Arrastar e soltar nós
- Conecta tudo: WhatsApp + banco + IA

---

### 🔄 TRANSIÇÃO: Slides → Terminal
> "Agora vamos sair da teoria e ir pro Antigravity. Vou abrir meu terminal e vocês vão ver a IA trabalhando ao vivo."

---

### BLOCO 3 — Projeto ao Vivo (60-90 min)
**Tela:** Terminal (Claude Code) + Navegador (n8n + Supabase)

#### Fase 1: Setup do Banco (10 min)

**No Claude Code, pedir:**
```
Cria a tabela group_messages no Supabase para armazenar mensagens de grupos WhatsApp.
Campos: id, group_id, group_name, sender_name, sender_number, message_text, message_type, timestamp, processed
```

**SQL esperado:**
```sql
CREATE TABLE group_messages (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  group_id TEXT NOT NULL,
  group_name TEXT,
  sender_name TEXT,
  sender_number TEXT,
  message_text TEXT,
  message_type TEXT DEFAULT 'text',
  timestamp TIMESTAMPTZ DEFAULT NOW(),
  processed BOOLEAN DEFAULT FALSE
);
```

- [ ] Tabela criada
- [ ] Verificar no Supabase que apareceu

#### Fase 2: Workflow 1 — Coletor de Mensagens (25 min)

**Passo a passo no n8n:**

1. **Criar workflow** "Coletor Resumo WhatsApp"
2. **Webhook node** — copiar URL do webhook
3. **Configurar na Evolution API:**
   - Ir nas configurações da instância
   - Adicionar webhook URL do n8n
   - Evento: `MESSAGES_UPSERT`
4. **IF node** — Filtrar mensagens:
   ```
   {{ $json.data.key.fromMe }} === false
   ```
5. **Code node** — Extrair dados:
   ```javascript
   const data = $input.first().json.data;
   return {
     group_id: data.key.remoteJid,
     group_name: data.pushName || 'Grupo',
     sender_name: data.pushName,
     sender_number: data.key.participant,
     message_text: data.message?.conversation ||
                   data.message?.extendedTextMessage?.text || '',
     message_type: 'text',
     timestamp: new Date().toISOString()
   };
   ```
6. **Supabase/HTTP node** — Insert no banco
7. **Ativar workflow**

**Teste:**
- [ ] Mandar mensagem no grupo WhatsApp
- [ ] Verificar que chegou no Supabase

#### Fase 3: Workflow 2 — Gerador de Resumo (25 min)

**Passo a passo no n8n:**

1. **Schedule Trigger** — Cron: `0 20 * * *` (20h todo dia)
   - Para teste: trigger manual
2. **Supabase/HTTP node** — Buscar mensagens do dia:
   ```sql
   SELECT * FROM group_messages
   WHERE timestamp >= CURRENT_DATE
   AND processed = false
   ORDER BY timestamp
   ```
3. **Code node** — Agrupar por grupo:
   ```javascript
   const messages = $input.all().map(i => i.json);
   const groups = {};

   for (const msg of messages) {
     if (!groups[msg.group_name]) {
       groups[msg.group_name] = [];
     }
     groups[msg.group_name].push(
       `${msg.sender_name}: ${msg.message_text}`
     );
   }

   return Object.entries(groups).map(([name, msgs]) => ({
     group_name: name,
     messages: msgs.join('\n'),
     count: msgs.length
   }));
   ```
4. **HTTP Request node** — Claude API:
   - URL: `https://api.anthropic.com/v1/messages`
   - Method: POST
   - Headers: `x-api-key: SUA_KEY`, `anthropic-version: 2023-06-01`
   - Body:
   ```json
   {
     "model": "claude-sonnet-4-20250514",
     "max_tokens": 1024,
     "messages": [{
       "role": "user",
       "content": "Resuma as mensagens deste grupo WhatsApp de forma concisa e organizada. Destaque os tópicos principais e decisões tomadas.\n\nGrupo: {{$json.group_name}}\nMensagens:\n{{$json.messages}}"
     }]
   }
   ```
5. **Evolution API node** — Enviar resumo via WhatsApp
6. **Supabase node** — Marcar como `processed = true`

**Teste:**
- [ ] Rodar workflow manualmente
- [ ] Verificar resumo recebido no WhatsApp

#### Fase 4: Teste End-to-End (10 min)
- [ ] Mandar várias mensagens no grupo
- [ ] Rodar workflow de resumo manualmente
- [ ] Mostrar o resumo chegando no WhatsApp
- [ ] "É isso que roda automaticamente todo dia às 20h"

---

### 🔄 TRANSIÇÃO: Terminal → Slides
> "Projeto funcionando! Vamos voltar pros slides pra recapitular."

---

### BLOCO 4 — Encerramento (15 min)
**Tela:** Slides

- Recapitular o que construíram
- Mostrar possibilidades: "Isso é só o começo"
  - Resumo com categorias
  - Alertas de tópicos importantes
  - Integração com Abacate Pay para cobranças
- Próximos passos
- Abrir para dúvidas

---

## Dicas para o Dia

### Timing
| Bloco | Duração | Horário (se 14h) |
|-------|---------|-------------------|
| Apresentação | 30 min | 14:00 - 14:30 |
| Stack Gratuita | 20 min | 14:30 - 14:50 |
| Intervalo | 10 min | 14:50 - 15:00 |
| Projeto ao vivo | 60-90 min | 15:00 - 16:30 |
| Encerramento | 15 min | 16:30 - 16:45 |

### Se algo der errado
- **Webhook não chega:** Verificar URL, verificar evento na Evolution API
- **Supabase erro de permissão:** Verificar API key e RLS policies (desativar RLS para demo)
- **Claude API erro:** Verificar API key e créditos
- **n8n erro em node:** Usar o output do node anterior para debug

### Frases úteis
- "Não se preocupem em copiar tudo — vou compartilhar os templates depois"
- "O importante aqui é entender o FLUXO, não decorar cada passo"
- "Qualquer dúvida, joga no chat que eu respondo"
