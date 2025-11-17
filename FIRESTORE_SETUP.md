# Configuração do Firestore

## 📋 Secrets Necessários

Você precisa configurar as seguintes variáveis de ambiente (secrets) no Replit:

### 1. `FIREBASE_PROJECT_ID`
- **Descrição**: ID do seu projeto Firebase
- **Onde encontrar**: Firebase Console → Project Settings → Project ID
- **Exemplo**: `meu-projeto-12345`

### 2. `FIREBASE_CLIENT_EMAIL`
- **Descrição**: Email da conta de serviço do Firebase
- **Onde encontrar**: Firebase Console → Project Settings → Service Accounts → Generate New Private Key (no arquivo JSON baixado)
- **Exemplo**: `firebase-adminsdk-xxxxx@meu-projeto-12345.iam.gserviceaccount.com`

### 3. `FIREBASE_PRIVATE_KEY`
- **Descrição**: Chave privada da conta de serviço
- **Onde encontrar**: No arquivo JSON baixado (campo `private_key`)
- **IMPORTANTE**: Cole o valor completo incluindo as quebras de linha (`\n`)
- **Exemplo**: `-----BEGIN PRIVATE KEY-----\nMIIEvQIBADANBg...\n-----END PRIVATE KEY-----\n`

## 🔧 Como Obter as Credenciais

1. **Acesse o Firebase Console**: https://console.firebase.google.com
2. **Selecione seu projeto** (ou crie um novo)
3. **Vá para Project Settings** (ícone de engrenagem no topo)
4. **Clique na aba "Service Accounts"**
5. **Clique em "Generate New Private Key"**
6. **Salve o arquivo JSON** (NÃO compartilhe este arquivo!)
7. **Copie os valores do JSON** para as secrets do Replit:
   - `project_id` → `FIREBASE_PROJECT_ID`
   - `client_email` → `FIREBASE_CLIENT_EMAIL`
   - `private_key` → `FIREBASE_PRIVATE_KEY`

## 📝 Como Adicionar as Secrets no Replit

1. No painel lateral do Replit, procure por "Secrets" ou "Tools"
2. Clique em "Secrets"
3. Adicione cada secret uma por vez:
   - Clique em "+ New Secret"
   - Cole o **nome** da variável (ex: `FIREBASE_PROJECT_ID`)
   - Cole o **valor** correspondente
   - Clique em "Save"
4. Repita para todas as 3 variáveis

## 🔒 Regras de Segurança do Firestore

**⚠️ IMPORTANTE**: Este aplicativo usa o **Firebase Admin SDK**, que possui **privilégios administrativos completos** e **ignora todas as regras de segurança do Firestore**. As regras em `firestore.rules` são fornecidas apenas como referência caso você decida adicionar acesso direto de cliente (web/mobile) no futuro.

### Para esta aplicação server-side:
- ✅ A segurança é implementada nas rotas da API (server/routes.ts)
- ✅ A autenticação é gerenciada pelo servidor
- ✅ O Admin SDK tem acesso total ao Firestore
- ❌ As regras de segurança do Firestore **NÃO** afetam o servidor

### Se você adicionar clientes web/mobile no futuro:

As regras de segurança estão no arquivo `firestore.rules`.

**Como aplicar as regras**:

1. **Via Firebase Console**:
   - Vá para Firestore Database → Rules
   - Cole o conteúdo do arquivo `firestore.rules`
   - Clique em "Publish"

2. **Via Firebase CLI** (se preferir):
   ```bash
   # Instale o Firebase CLI
   npm install -g firebase-tools
   
   # Faça login
   firebase login
   
   # Inicialize (se ainda não fez)
   firebase init firestore
   
   # Deploy das regras
   firebase deploy --only firestore:rules
   ```

## 🎯 Estrutura de Coleções

Após configurar, o Firestore terá as seguintes coleções:

- `users` - Usuários do sistema
- `cartinhas` - Cartinhas criadas
- `photos` - Fotos compartilhadas
- `photoComments` - Comentários nas fotos
- `postits` - Post-its na página de mensagens
- `notes` - Notas criadas
- `drawings` - Desenhos salvos
- `touches` - Registros de toques no contador
- `contador` - Configuração do contador

## 🚀 Inicialização do Banco de Dados

Após adicionar as secrets, você precisa popular o banco de dados com dados iniciais:

```bash
npx tsx scripts/seed-firestore.ts
```

Este script irá:
- Criar 2 usuários iniciais (jessica e miguel) com senha "jem1505"
- Inicializar o contador com a data 15/05/2024

### Migração de Dados Existentes (Opcional)

Se você já tem dados no Firestore de uma implementação anterior, execute este script para garantir que todos os documentos tenham os campos de timestamp necessários:

```bash
npx tsx scripts/migrate-timestamps.ts
```

Este script irá:
- Verificar todas as coleções (postits, notes, photos, etc.)
- Adicionar campos `createdAt` e `updatedAt` ausentes
- Evitar crashes ao ler documentos antigos

## ✅ Verificação

Após adicionar as secrets e executar o seed, a aplicação deve:
1. Conectar ao Firestore automaticamente
2. Iniciar sem erros
3. Permitir login com os usuários criados
4. Salvar dados nas coleções do Firestore

## 🗑️ Limpeza de Arquivos Antigos (PostgreSQL)

Os seguintes arquivos foram substituídos pela implementação do Firestore e podem ser removidos se não forem mais necessários:

- `server/db.ts` - Configuração do PostgreSQL
- `drizzle.config.ts` - Configuração do Drizzle ORM
- `scripts/init-db.ts` - Script de inicialização do PostgreSQL
- `scripts/seed.ts` - Seed antigo do PostgreSQL

**Pacotes que podem ser removidos**:
- `@neondatabase/serverless`
- `drizzle-orm`
- `drizzle-kit`
- `ws` (se não usado em outro lugar)
