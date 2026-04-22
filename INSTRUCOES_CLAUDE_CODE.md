# INSTRUÇÕES PARA O CLAUDE CODE — PROJETO VINNO
# Leia este arquivo inteiro antes de executar qualquer comando.

## O QUE É ESTE PROJETO

O Vinno é um app de vinhos com 3 partes:

1. /app/vinno.html        — protótipo do app (HTML completo, funcional)
2. /landing/index.html    — landing page de lista de espera pré-lançamento
3. /backend/              — API Node.js para Vercel + Supabase (22 rotas)

---

## O QUE VOCÊ PRECISA FAZER

### PARTE 1 — Publicar o backend no GitHub

1. Verificar se o Git está instalado (git --version)
   - Se não estiver: instalar conforme o sistema operacional do usuário

2. Verificar se o Node.js está instalado (node --version)
   - Precisa ser versão 18 ou superior

3. Pedir ao usuário para criar uma conta no GitHub em https://github.com
   - Se ainda não tiver

4. Configurar o Git com os dados do usuário:
   git config --global user.name "Nome do usuário"
   git config --global user.email "email do usuário"

5. Criar repositório no GitHub chamado "vinno-backend"
   - Orientar o usuário a criar em https://github.com/new
   - Sem README, sem .gitignore (já temos os nossos)

6. Dentro da pasta /backend/, inicializar e publicar:
   cd /path/para/esta/pasta/backend
   git init
   git add .
   git commit -m "feat: backend Vinno — 22 rotas Vercel + Supabase"
   git branch -M main
   git remote add origin https://github.com/USERNAME/vinno-backend.git
   git push -u origin main

### PARTE 2 — Publicar o app e landing no GitHub

1. Criar repositório no GitHub chamado "vinno-app"

2. Dentro da pasta raiz do projeto (onde estão /app e /landing):
   git init
   git add .
   git commit -m "feat: app Vinno e landing page"
   git branch -M main
   git remote add origin https://github.com/USERNAME/vinno-app.git
   git push -u origin main

### PARTE 3 — Configurar a Vercel (deploy do backend)

1. Verificar se a Vercel CLI está instalada:
   vercel --version

2. Se não estiver, instalar:
   npm install -g vercel

3. Fazer login na Vercel:
   vercel login

4. Dentro da pasta /backend/, fazer o deploy:
   cd /path/para/esta/pasta/backend
   npm install
   vercel

   Responder as perguntas:
   - "Set up and deploy?" → Y
   - "Which scope?" → conta do usuário
   - "Link to existing project?" → N
   - "Project name?" → vinno-backend
   - "In which directory is your code?" → . (ponto)
   - "Override settings?" → N

5. Configurar as variáveis de ambiente (pedir as chaves ao usuário):
   vercel env add ANTHROPIC_API_KEY
   vercel env add SUPABASE_URL
   vercel env add SUPABASE_SERVICE_KEY

   Para cada uma: selecionar todos os ambientes (Production, Preview, Development)

6. Deploy final com as variáveis:
   vercel --prod

7. Confirmar que funcionou testando a rota de busca:
   curl -X POST https://vinno-backend.vercel.app/api/ai/search \
     -H "Content-Type: application/json" \
     -d '{"query": "Malbec"}'

---

## VARIÁVEIS DE AMBIENTE NECESSÁRIAS

Antes do deploy, o usuário precisa ter:

- ANTHROPIC_API_KEY  → https://console.anthropic.com → API Keys
- SUPABASE_URL       → Supabase → Settings → API → Project URL
- SUPABASE_SERVICE_KEY → Supabase → Settings → API → service_role (secret)

O arquivo .env.example na pasta /backend/ tem o modelo completo.

---

## BANCO DE DADOS (Supabase)

Antes do deploy, o usuário precisa:

1. Criar conta em https://supabase.com
2. Criar projeto chamado "vinno" na região South America (São Paulo)
3. Ir em SQL Editor → New Query
4. Colar o conteúdo de /backend/supabase/schema.sql
5. Clicar em Run

---

## ESTRUTURA DO BACKEND (para referência)

backend/
├── api/
│   ├── ai/
│   │   ├── search.js      POST /api/ai/search      busca vinho com IA
│   │   ├── describe.js    POST /api/ai/describe    descrição sensorial
│   │   └── harmonize.js   POST /api/ai/harmonize   harmonização por prato
│   ├── auth/
│   │   ├── me.js          GET  /api/auth/me         dados do usuário
│   │   └── profile.js     GET/PATCH /api/auth/profile  perfil
│   ├── wines/
│   │   ├── save.js        POST /api/wines/save      salvar degustação
│   │   ├── list.js        GET  /api/wines/list      listar vinhos
│   │   └── cellar.js      POST/PATCH/DELETE /api/wines/cellar  adega
│   ├── posts/
│   │   ├── index.js       GET/POST /api/posts       feed social
│   │   └── [id]/
│   │       ├── like.js    POST/DELETE /api/posts/:id/like
│   │       └── comment.js GET/POST   /api/posts/:id/comment
│   ├── follows/
│   │   └── index.js       GET/POST/DELETE /api/follows
│   ├── upload.js          POST /api/upload          fotos
│   └── waitlist.js        POST /api/waitlist        lista de espera
├── lib/
│   ├── supabase.js        cliente Supabase
│   ├── cors.js            headers CORS
│   └── rateLimit.js       proteção contra abuso
├── supabase/
│   └── schema.sql         schema completo do banco
├── .env.example           modelo das variáveis de ambiente
├── package.json
└── vercel.json

---

## SE ALGO DER ERRADO

- Erro "git: command not found" → instalar Git em git-scm.com
- Erro "npm: command not found" → instalar Node.js em nodejs.org
- Erro de autenticação no GitHub → rodar: git credential-manager configure
- Erro 401 na Vercel → rodar: vercel login novamente
- Erro no push → verificar se a URL do remote está correta: git remote -v
