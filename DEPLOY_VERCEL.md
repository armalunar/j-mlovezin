# Como fazer Deploy no Vercel - Site J&M

## Pré-requisitos
1. Conta no Vercel (gratuita): https://vercel.com
2. GitHub instalado (opcional, mas recomendado)

## Opção 1: Deploy via GitHub (Recomendado)

### Passo 1: Criar repositório no GitHub
1. Vá em https://github.com/new
2. Crie um novo repositório (pode ser privado)
3. NÃO inicialize com README

### Passo 2: Fazer push do código para GitHub
No terminal do Replit, execute:

```bash
git init
git add .
git commit -m "Initial commit - Site J&M"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/SEU_REPOSITORIO.git
git push -u origin main
```

### Passo 3: Importar no Vercel
1. Vá em https://vercel.com/new
2. Clique em "Import Git Repository"
3. Selecione seu repositório do GitHub
4. Configure:
   - **Framework Preset**: Other
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist/public`
   - **Install Command**: `npm install`

### Passo 4: Configurar Variáveis de Ambiente
No painel do Vercel, vá em Settings > Environment Variables e adicione:

```
NODE_ENV=production
JWT_SECRET=seu_secret_super_seguro_aqui_123456
```

**IMPORTANTE**: Se quiser usar PostgreSQL na produção:
- Crie um banco no Vercel Postgres ou Neon
- Adicione a variável `DATABASE_URL` com a string de conexão

### Passo 5: Deploy
1. Clique em "Deploy"
2. Aguarde o build (2-3 minutos)
3. Seu site estará no ar! 🎉

## Opção 2: Deploy via CLI do Vercel

### Passo 1: Instalar Vercel CLI
```bash
npm install -g vercel
```

### Passo 2: Login
```bash
vercel login
```

### Passo 3: Deploy
```bash
vercel
```

Siga as instruções interativas.

## Opção 3: Deploy Manual (mais simples)

### Passo 1: Baixar o projeto
1. No Replit, clique nos 3 pontos no topo
2. Selecione "Download as zip"
3. Extraia o arquivo zip no seu computador

### Passo 2: Deploy no Vercel
1. Vá em https://vercel.com/new
2. Arraste a pasta do projeto
3. Configure as mesmas opções da Opção 1
4. Clique em Deploy

## Após o Deploy

### Acessar o site
- Seu site estará em: `https://seu-projeto.vercel.app`
- Você pode adicionar um domínio customizado nas configurações

### Login
- Usuários: `jessica` e `miguel`
- Senha: `jem1505`

### Configurar Domínio Customizado (Opcional)
1. No painel do Vercel, vá em Settings > Domains
2. Adicione seu domínio
3. Configure o DNS conforme as instruções

## Notas Importantes

### 🔴 Limitações do Vercel (Plano Gratuito)
- **WebSockets**: Não funcionam no Vercel! O desenho colaborativo em tempo real não vai funcionar
- **Persistência**: Sem banco de dados, os dados são perdidos a cada deploy
- **Tempo de execução**: Máximo de 10 segundos por request

### ✅ Soluções

#### Para WebSockets
Opções:
1. **Heroku** (melhor para WebSockets, mas pago)
2. **Render.com** (grátis, suporta WebSockets)
3. **Railway** (grátis, suporta WebSockets)

#### Para Banco de Dados (Persistência)
1. **Vercel Postgres** (integração perfeita, pago)
2. **Neon** (PostgreSQL grátis): https://neon.tech
3. **Supabase** (PostgreSQL grátis): https://supabase.com

## Alternativas ao Vercel

### Render.com (RECOMENDADO para este projeto)
- ✅ Suporta WebSockets
- ✅ Plano gratuito
- ✅ PostgreSQL gratuito
- ✅ Fácil de configurar

Como fazer deploy no Render:
1. Vá em https://render.com
2. Crie uma conta
3. New > Web Service
4. Conecte seu repositório GitHub
5. Configure:
   - **Build Command**: `npm install && npm run build`
   - **Start Command**: `npm start`
   - **Environment**: Node
6. Adicione as variáveis de ambiente
7. Deploy!

### Heroku
- ✅ Suporta WebSockets
- ✅ PostgreSQL incluído
- ❌ Não tem plano gratuito mais

### Railway
- ✅ Suporta WebSockets
- ✅ PostgreSQL incluído
- ✅ $5 grátis/mês

## Recomendação Final

Para o site J&M com todas as funcionalidades (incluindo desenho colaborativo), recomendo:

**🎯 Render.com + Neon PostgreSQL**
- 100% gratuito
- Suporta todas as funcionalidades
- Fácil de configurar
- Domínio customizado grátis

## Suporte
Se tiver problemas, verifique:
- Logs do Vercel/Render
- Variáveis de ambiente configuradas
- Build completado com sucesso
