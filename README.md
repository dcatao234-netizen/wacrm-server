# WA CRM Server v4 — Guia de Deploy

## Passo a passo no Railway

### 1. Criar o projeto
- Acesse railway.app → New Project

### 2. Subir a Evolution API
- Clique "+ Add" → Template → busque "Evolution API"
- Ou: Docker Image → `evoapicloud/evolution-api:latest`
- Variáveis necessárias:
  - PORT=8080
  - AUTHENTICATION_API_KEY=minhachave2025
  - DATABASE_PROVIDER=postgresql
  - DATABASE_CONNECTION_URI=${Postgres.DATABASE_URL}
  - CACHE_REDIS_URI=${Redis.REDIS_URL}
  - CACHE_REDIS_ENABLED=true
  - SERVER_URL=https://${RAILWAY_PUBLIC_DOMAIN}

### 3. Adicionar Redis
- "+ Add" → Database → Redis

### 4. Adicionar Postgres
- "+ Add" → Database → PostgreSQL

### 5. Subir este servidor (wacrm-server)
- "+ Add" → GitHub Repository → selecione wacrm-server
- Variáveis necessárias (Settings → Variables):
  - EVOLUTION_URL=https://SUA-EVOLUTION-API.up.railway.app
  - EVOLUTION_KEY=minhachave2025
  - INSTANCE_NAME=meu-whatsapp
  - DATABASE_URL → Add Reference → Postgres → DATABASE_URL
  - MY_URL=https://SEU-WACRM-SERVER.up.railway.app

### 6. Gerar domínio público
- Cada serviço: Settings → Domains → Generate Domain

### 7. Conectar WhatsApp
- Acesse: https://SUA-EVOLUTION-API.up.railway.app/manager
- API Key: minhachave2025
- Criar instância: meu-whatsapp
- Escanear QR Code

### 8. Atualizar o CRM (Netlify)
- No index.html, atualize as URLs:
  - CFG.evoUrl    = 'https://SUA-EVOLUTION-API.up.railway.app'
  - CFG.serverUrl = 'https://SEU-WACRM-SERVER.up.railway.app'
- Suba novamente na Netlify

## Estrutura do banco de dados

- **conversations** — todos os leads/contatos
- **messages** — histórico completo de mensagens
- **quick_messages** — mensagens rápidas do CRM

## Endpoints principais

| Método | Rota | Descrição |
|--------|------|-----------|
| GET | / | Health check |
| GET | /db/conversas | Carrega todos os leads do banco |
| POST | /db/conversas | Salva/atualiza um lead |
| POST | /db/mensagens | Salva mensagens no banco |
| POST | /enviar | Envia mensagem de texto |
| POST | /enviar-midia | Envia imagem/áudio |
| POST | /webhook | Recebe mensagens em tempo real |
| POST | /configurar-webhook | Configura webhook na Evolution API |
