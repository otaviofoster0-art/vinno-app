# Vinno — Projeto Completo

App de vinhos com adega virtual, IA e social.

## Estrutura

```
vinno-projeto/
├── app/
│   └── vinno.html              ← protótipo completo do app
├── landing/
│   └── index.html              ← landing page (lista de espera)
├── backend/                    ← API para Vercel + Supabase
│   ├── api/                    ← 22 rotas
│   ├── lib/                    ← helpers (cors, supabase, rate limit)
│   ├── supabase/schema.sql     ← banco de dados completo
│   ├── .env.example            ← variáveis de ambiente necessárias
│   └── DEPLOY.md               ← guia de deploy passo a passo
└── INSTRUCOES_CLAUDE_CODE.md   ← instruções para o Claude Code
```

## Deploy rápido

1. Leia `INSTRUCOES_CLAUDE_CODE.md`
2. Configure o Supabase com `backend/supabase/schema.sql`
3. Siga `backend/DEPLOY.md` para subir na Vercel

## Tecnologias

- Frontend: HTML + CSS + JS puro (sem framework)
- Backend: Node.js serverless (Vercel)
- Banco: PostgreSQL (Supabase)
- IA: Claude API (Anthropic)
- Auth: Supabase Auth
- Storage: Supabase Storage
