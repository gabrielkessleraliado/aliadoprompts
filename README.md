# Aliado — Sistema de Prompts v2.0

Pipeline de análise de atendimentos com QA automático.

## Deploy no Vercel (5 minutos)

### 1. Pré-requisitos
- Conta gratuita no [Vercel](https://vercel.com) (login com GitHub/Google)
- Sua API Key da Anthropic — obtenha em [console.anthropic.com](https://console.anthropic.com)

### 2. Deploy via Vercel CLI (opção A — linha de comando)

```bash
# Instalar a CLI do Vercel (se não tiver)
npm install -g vercel

# Entrar na pasta do projeto
cd aliado-vercel

# Fazer o deploy
vercel

# Quando perguntar "Set up and deploy?" → Y
# Project name → aliado-prompts (ou qualquer nome)
# Directory → ./ (raiz)
```

### 3. Deploy via GitHub (opção B — sem linha de comando)

1. Crie um repositório no GitHub e suba esta pasta
2. Acesse [vercel.com](https://vercel.com) → "New Project"
3. Importe o repositório
4. Clique em "Deploy"

### 4. Configurar a API Key (obrigatório)

Após o deploy:
1. Vá em **Settings → Environment Variables** no painel do projeto no Vercel
2. Adicione:
   - **Name:** `ANTHROPIC_API_KEY`
   - **Value:** sua chave (começa com `sk-ant-...`)
   - **Environment:** Production + Preview + Development
3. Clique em **Save**
4. Vá em **Deployments** → clique nos três pontos do último deploy → **Redeploy**

Pronto. A URL gerada pelo Vercel (ex: `https://aliado-prompts.vercel.app`) funciona para qualquer pessoa, sem instalar nada.

## Estrutura do projeto

```
aliado-vercel/
├── api/
│   └── chat.js          # Proxy serverless — recebe chamadas do browser e
│                        # repassa para a Anthropic com a API key segura
├── public/
│   └── index.html       # Frontend completo (pipeline P0→P5b)
└── vercel.json          # Configuração de rotas
```

## Segurança

- A API key **nunca** fica exposta no browser — fica apenas nas variáveis de ambiente do Vercel
- O proxy `/api/chat` aceita qualquer origem (`*`). Se quiser restringir ao seu domínio,
  troque `'*'` por `'https://seu-dominio.vercel.app'` no arquivo `api/chat.js`

## Custos estimados

- Vercel: **gratuito** (plano Hobby — 100GB bandwidth/mês, funções serverless incluídas)
- Anthropic: ~$0.003 por análise completa (7 chamadas × claude-sonnet-4-6)
