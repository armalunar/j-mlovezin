# 🚀 Guia de Deploy no Render.com

Este guia completo mostra como fazer o deploy desta aplicação no Render.com com banco de dados PostgreSQL.

## 📋 Pré-requisitos

- Conta no [Render.com](https://render.com) (pode ser gratuita)
- Conta no [GitHub](https://github.com)
- Projeto baixado como ZIP do Replit

---

## 1️⃣ Baixar o Projeto do Replit

1. No Replit, clique nos **três pontos** (⋮) no topo do explorador de arquivos
2. Selecione **"Download as zip"**
3. Salve o arquivo ZIP no seu computador
4. Extraia o conteúdo do ZIP em uma pasta

---

## 2️⃣ Fazer Upload para o GitHub

### Opção A: Através do GitHub Desktop (Recomendado para iniciantes)

1. Baixe e instale o [GitHub Desktop](https://desktop.github.com/)
2. Crie um novo repositório:
   - File → New Repository
   - Name: `meu-projeto` (ou o nome que preferir)
   - Local Path: Selecione a pasta onde extraiu o ZIP
   - Clique em **Create Repository**
3. Faça o commit inicial:
   - Escreva uma mensagem: "Initial commit"
   - Clique em **Commit to main**
4. Publique no GitHub:
   - Clique em **Publish repository**
   - Desmarque "Keep this code private" se quiser público
   - Clique em **Publish Repository**

### Opção B: Através da linha de comando (Git)

```bash
cd caminho/para/pasta/do/projeto
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/seu-usuario/seu-repositorio.git
git push -u origin main
```

---

## 3️⃣ Criar Banco de Dados PostgreSQL no Render

1. Acesse [Render Dashboard](https://dashboard.render.com/)
2. Clique em **New +** → **PostgreSQL**
3. Configure o banco:
   - **Name**: `meu-projeto-db` (ou outro nome)
   - **Database**: deixe em branco (auto-gerado)
   - **User**: deixe em branco (auto-gerado)
   - **Region**: escolha a região mais próxima (ex: `Oregon (US West)`)
   - **PostgreSQL Version**: deixe padrão
   - **Plan**: selecione **Free** (expira em 90 dias, mas é gratuito)
4. Clique em **Create Database**
5. ⚠️ **IMPORTANTE**: Após criar, copie a **Internal Database URL** da seção **Connections**
   - Ela será algo como: `postgresql://user:password@dpg-xxx.oregon-postgres.render.com/database_xxx`
   - Guarde esta URL, você vai precisar dela no próximo passo!

---

## 4️⃣ Criar Web Service no Render

1. No Render Dashboard, clique em **New +** → **Web Service**
2. Conecte seu repositório GitHub:
   - Clique em **Connect** no repositório que você criou
   - Se não aparecer, clique em **Configure account** para autorizar o Render
3. Configure o serviço:

   **Configurações Básicas:**
   - **Name**: `meu-projeto` (será seu domínio: `meu-projeto.onrender.com`)
   - **Region**: ⚠️ **DEVE SER A MESMA do banco de dados!**
   - **Branch**: `main`
   - **Root Directory**: deixe vazio (`.`)
   - **Environment**: `Node`

   **Build & Deploy:**
   - **Build Command**: 
     ```
     npm install && npm run build
     ```
   - **Start Command**:
     ```
     npm start
     ```

4. Clique em **Advanced** e adicione as **Environment Variables** (TODAS SÃO OBRIGATÓRIAS):

   | Key | Value | Descrição |
   |-----|-------|-----------|
   | `DATABASE_URL` | `postgresql://user:...` | Cole a Internal Database URL do passo 3 |
   | `NODE_ENV` | `production` | Ambiente de produção |
   | `PORT` | `10000` | Porta padrão do Render |
   | `SESSION_SECRET` | `sua-chave-aleatoria-aqui` | Chave secreta para sessões (gere uma string aleatória longa e segura) |

   **Como gerar SESSION_SECRET segura:**
   - Opção 1: Use um gerador online de strings aleatórias (min. 32 caracteres)
   - Opção 2: No terminal, execute: `node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"`
   - Exemplo de valor: `a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6`

5. **Plan**: Selecione **Free**

6. Clique em **Create Web Service**

---

## 5️⃣ Executar Migrations do Banco de Dados

Após o deploy inicial, você precisa criar as tabelas no banco:

1. No dashboard do seu Web Service no Render, vá até a aba **Shell**
2. Execute o comando para criar as tabelas:
   ```bash
   npm run db:push -- --force
   ```
3. Aguarde a confirmação de que as tabelas foram criadas

**Alternativa**: Você pode adicionar este comando ao Build Command:
```
npm install && npm run build && npm run db:push -- --force
```

---

## 6️⃣ Verificar Deploy

1. Aguarde o build completar (pode levar alguns minutos)
2. Quando terminar, você verá **"Live"** em verde
3. Clique no link `https://meu-projeto.onrender.com` para abrir sua aplicação
4. Teste as funcionalidades:
   - ✅ Login/Registro de usuários
   - ✅ Criação de notas (com botão Salvar!)
   - ✅ Criação de post-its (aparecem na tela!)
   - ✅ Desenhos na tela
   - ✅ Timeline com posts

---

## 🔄 Deploy Automático

Sempre que você fizer alterações no código:

1. Faça commit e push para o GitHub:
   ```bash
   git add .
   git commit -m "Descrição das mudanças"
   git push
   ```
2. O Render detectará automaticamente e fará o deploy das mudanças!

---

## ⚙️ Configurações Importantes do Projeto

Este projeto já está configurado para funcionar no Render:

### ✅ Porta Dinâmica
```javascript
// server/index.ts
const port = parseInt(process.env.PORT || '5000', 10);
```

### ✅ SSL do PostgreSQL
```javascript
// server/db.ts
ssl: isProduction ? { rejectUnauthorized: false } : false
```

### ✅ Scripts de Build
```json
// package.json
{
  "scripts": {
    "build": "vite build && esbuild server/index.ts ...",
    "start": "NODE_ENV=production node dist/index.js"
  }
}
```

---

## 🐛 Resolução de Problemas

### Erro de conexão com o banco
- ✅ Verifique se `DATABASE_URL` está correta
- ✅ Confirme que Web Service e Database estão na **mesma região**
- ✅ Certifique-se que executou `npm run db:push -- --force`

### App não inicia
- ✅ Verifique os logs na aba **Logs** do Render
- ✅ Confirme que `NODE_ENV=production` está configurado
- ✅ Verifique se o Build Command completou com sucesso

### Free Tier se desliga
- ⚠️ O tier gratuito do Render desliga após 15 minutos de inatividade
- A primeira requisição após isso levará ~30 segundos para "acordar" o serviço
- Isso é normal no plano gratuito!

### Banco de dados expira
- ⚠️ O PostgreSQL gratuito expira após 90 dias
- Você precisará criar um novo banco e migrar os dados
- Considere fazer backup dos dados importantes

---

## 📝 Checklist Final

Antes de considerar o deploy completo, verifique:

- [ ] Banco de dados PostgreSQL criado no Render
- [ ] Internal Database URL copiada
- [ ] Repositório no GitHub criado e publicado
- [ ] Web Service criado no Render
- [ ] Variáveis de ambiente configuradas (`DATABASE_URL`, `NODE_ENV`, `PORT`, `SESSION_SECRET`)
- [ ] SESSION_SECRET gerada de forma segura (min. 32 caracteres aleatórios)
- [ ] Web Service e Database na mesma região
- [ ] Migrations executadas (`npm run db:push -- --force`)
- [ ] Build completado com sucesso
- [ ] Aplicação acessível via URL do Render
- [ ] Funcionalidades testadas (login, notas, post-its)

---

## 🎉 Pronto!

Sua aplicação está no ar! Compartilhe o link `https://seu-projeto.onrender.com` com quem quiser.

### Funcionalidades implementadas:
- ✅ **Notas** agora têm botão de salvar manual
- ✅ **Post-its** aparecem sempre em posições visíveis
- ✅ **Banco de dados** configurado para persistir dados entre todos os usuários
- ✅ **Deploy** otimizado para Render.com

---

## 📞 Suporte

Se encontrar problemas:
1. Verifique os logs no Render Dashboard
2. Consulte a [documentação oficial do Render](https://render.com/docs)
3. Revise este guia passo a passo

Bom deploy! 🚀
