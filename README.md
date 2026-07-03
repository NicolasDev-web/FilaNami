# FilaNami

FilaNami é uma aplicação de gerenciamento de filas de atendimento com API REST (backend) e interface web (frontend). Foi desenvolvida para controlar guichês, senhas e histórico de atendimentos, oferecendo endpoints para autenticação e operações administrativas. Ideal para pequenas e médias secretarias, clínicas ou balcões de atendimento.

## Conteúdo
- Visão geral
- Stack
- Estrutura do repositório
- Como rodar (desenvolvimento e produção)
- Banco de dados (Prisma)
- Coleção Postman
- Boas práticas e contribuição
- Licença e contato

---

## Visão geral
FilaNami oferece:
- Gestão de senhas (emissão e andamento)
- Controle de guichês/setores
- Histórico de atendimentos
- Autenticação de usuários e guichês

Backend em Node.js com ORM Prisma; frontend com Vite (HTML/CSS/JS). Há uma coleção Postman inclusa para testar a API.

### Stack
- Linguagens: JavaScript, CSS, HTML
- Backend: Node.js (API REST) + Prisma ORM
- Frontend: Vite (dev server / build)
- Notáveis: Prisma (modelo e migrations), Postman collection (postmanApi.json)

---

## Como o repositório está organizado
Raiz:
```
backend/         API REST, Prisma, código do servidor (src/, prisma/, package.json)
frontend/        App web (Vite, src/, public/, index.html, package.json)
postmanApi.json  Coleção Postman para testar a API
README.md        (este arquivo)
LICENSE          Licença do projeto
```

backend/src (principais diretórios):
```
src/
  controllers/    Lógica das rotas (guicheController, senhaController, userController, historicoController, etc.)
  routes/         Rotas que expõem a API (guicheRoute, senhaRoute, userRoute, ...)
  repositories/   DAO / acesso ao banco (guicheDao, senhaDao, userDao, ...)
  middlewares/    Middlewares (ex.: authMiddleware.js)
  prisma.js       Inicialização do cliente Prisma
  server.js       Ponto de entrada do servidor
  app.js          Configuração do app/Express (separação de app/server)
```

backend/prisma:
- schema.prisma — modelo de dados
- migrations/ — migrations geradas (quando aplicadas)

frontend:
```
src/             Código da interface (componentes/scripts)
public/          Recursos estáticos
index.html       Entrada da aplicação web
vite.config.js   Configuração do Vite
```

Como se encaixa: O frontend consome a API REST (endpoints definidos em backend/src/routes). O backend usa Prisma para persistência; as controllers orquestram lógica e usam repositories/DAOs para consultas.

---

## Pré-requisitos
- Node.js (versão LTS recomendada)
- npm (ou yarn)
- Um banco compatível com Prisma (ex.: PostgreSQL, MySQL, SQLite). Configure via DATABASE_URL no .env
- Git

---

## Instalação e execução (desenvolvimento)

1. Clone o repositório
```bash
git clone https://github.com/NicolasDev-web/FilaNami.git
cd FilaNami
```

2. Backend
```bash
cd backend
cp .env.example .env         # ajustar variáveis conforme necessário
npm install
# Gerar client Prisma (necessário sempre após alterar schema)
npx prisma generate
# Rodar migrations (desenvolvimento)
npx prisma migrate dev --name init

# Iniciar em modo desenvolvimento (verifique os scripts em backend/package.json)
npm run dev
# ou, se existir:
# npm start
```

3. Frontend
```bash
cd ../frontend
cp .env.example .env         # ajustar variáveis (ex.: URL da API)
npm install

# Iniciar servidor de desenvolvimento (Vite)
npm run dev

# Build para produção
npm run build
```

Observações:
- Verifique os scripts disponíveis em `backend/package.json` e `frontend/package.json` para confirmar os comandos `start`, `dev`, `build` usados no projeto.
- A porta do servidor e outras variáveis (ex.: DATABASE_URL, JWT secret) estão nas `.env.example`. Copie e ajuste conforme o ambiente.

---

## Banco de dados (Prisma)
O projeto usa Prisma. Arquivo principal: `backend/prisma/schema.prisma`.

Comandos úteis:
- Gerar client: `npx prisma generate`
- Criar/rodar migrations (dev): `npx prisma migrate dev --name <nome>`
- Aplicar migrations (prod): `npx prisma migrate deploy`
- Abrir Prisma Studio: `npx prisma studio`

Assegure que `DATABASE_URL` em `backend/.env` aponte para o banco de dados desejado.

---

## API e coleção Postman
Há uma coleção Postman no arquivo `postmanApi.json` na raiz do repositório. Importe-a no Postman para testar endpoints (autenticação, guichês, senhas, histórico).

Dicas:
- Primeiro crie um usuário/conta via endpoint de registro (se existir) ou use credenciais de seed.
- Use o endpoint de login para obter token (se a API usa JWT) e inclua-o no header Authorization nas requisições protegidas.

## Deploy (resumo)
- Backend: publicar em um serviço Node (Heroku, Render, DigitalOcean App Platform, Railway), certificando-se de definir `DATABASE_URL` e outras variáveis de ambiente (PORT, JWT_SECRET).
- Frontend: build com `npm run build` e servir via CDN / serviço estático (Netlify, Vercel) ou integrado ao backend.
- Em produção, execute `npx prisma migrate deploy` para aplicar migrations e `npx prisma generate` para garantir o client.
  
## Licença
Consulte o arquivo LICENSE na raiz do repositório
