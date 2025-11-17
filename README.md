# J&M - Nosso Cantinho Especial ❤️

Um site romântico e interativo criado para Jessica e Miguel compartilharem momentos, memórias e amor.

## 🎨 Características

### Páginas Principais

- **Contador**: Tempo real desde 15/05/2024 18:37:54, com interação hover que registra toques
- **Cartinhas**: Upload e visualização de PDFs com thumbnails e mensagens especiais
- **Nossas Fotos**: Galeria de fotos com thumbnails, comentários em tempo real
- **Mensagens (Post-its)**: Mural interativo com post-its arrastáveis e customizáveis
- **Desenho Online**: Canvas colaborativo tipo paint com ferramentas de desenho
- **Notas**: Editor rich-text estilo Notion com Tiptap

### Funcionalidades Especiais

- ✨ **Dois Temas Personalizados**:
  - **Tema J** (Jéssica): Preto + Verde Neon + Rosa pastel
  - **Tema M** (Miguel): Preto + Vermelho profundo
- 🎵 **Player de Música Flutuante**: 3 músicas com controles completos
- 🎨 **Partículas Animadas**: Background dinâmico que se adapta ao tema
- ⚡ **Tempo Real**: WebSocket para sincronização de post-its e comentários
- 🔒 **Autenticação JWT**: Sistema seguro com cookies HttpOnly

## 🚀 Como Executar

### 1. Instalar Dependências

```bash
npm install
```

### 2. Configurar Variáveis de Ambiente

O projeto já está configurado com o banco de dados PostgreSQL do Replit. As seguintes variáveis já estão disponíveis:

- `DATABASE_URL` - URL de conexão do PostgreSQL
- `SESSION_SECRET` - Segredo para JWT (já configurado)

### 3. Criar Tabelas do Banco de Dados

```bash
npm run db:push
```

### 4. Popular Banco de Dados (Seed)

```bash
npx tsx scripts/seed.ts
```

Isso criará:
- **Usuário 1**: `jessica` / senha: `jem1505` (Tema J)
- **Usuário 2**: `miguel` / senha: `jem1505` (Tema M)
- **Contador** inicializado em 15/05/2024 18:37:54

### 5. Iniciar Aplicação

```bash
npm run dev
```

A aplicação estará disponível em `http://localhost:5000`

## 👤 Credenciais de Acesso

### Jéssica
- **Usuário**: `jessica`
- **Senha**: `jem1505`
- **Tema Padrão**: J (Verde Neon + Rosa)

### Miguel
- **Usuário**: `miguel`
- **Senha**: `jem1505`
- **Tema Padrão**: M (Vermelho)

## 📁 Estrutura do Projeto

```
j&m/
├── client/               # Frontend React + Vite
│   ├── public/
│   │   ├── audio/       # Músicas MP3 (3 arquivos inclusos)
│   │   └── uploads/     # PDFs e imagens enviados pelos usuários
│   └── src/
│       ├── components/  # Componentes reutilizáveis
│       ├── pages/       # Páginas da aplicação
│       ├── lib/         # Contextos e utilitários
│       └── index.css    # Temas J e M
├── server/              # Backend Express + Socket.IO
│   ├── db.ts           # Configuração Drizzle
│   ├── storage.ts      # Camada de dados
│   └── routes.ts       # APIs REST + WebSocket
├── shared/
│   └── schema.ts       # Modelos de dados compartilhados
└── scripts/
    └── seed.ts         # Script de inicialização
```

## 🎵 Músicas Incluídas

O projeto inclui 3 músicas MP3 em `client/public/audio/`:
1. Golden Hour
2. Jigsaw Falling Into Place
3. Snowfall

## 📤 Upload de Arquivos

### PDFs (Cartinhas)
- Faça upload através da página "Cartinhas"
- Tamanho máximo: configurável no multer
- Thumbnails gerados automaticamente

### Imagens (Fotos)
- Faça upload através da página "Nossas Fotos"
- Redimensionamento automático para otimização
- Thumbnails de 200x200px criados com Sharp

## 🎨 Temas

### Alternar Tema
Clique no ícone de paleta no header para alternar entre os temas J e M.

### Cores dos Temas

**Tema J (Jéssica)**:
- Base: Preto puro (#000000)
- Primária: Verde Neon (#00FF88)
- Secundária: Rosa Pastel (#FFB6C1)
- Partículas: Corações rosas e estrelas verdes

**Tema M (Miguel)**:
- Base: Preto puro (#000000)
- Primária: Vermelho (#DC2626)
- Secundária: Vermelho escuro (#7F1D1D)
- Partículas: Corações vermelhos e brasas

## 🔧 Tecnologias Utilizadas

### Frontend
- React 18
- TypeScript
- Tailwind CSS
- Shadcn/ui (componentes)
- Framer Motion (animações)
- Tiptap (editor rich-text)
- React Hook Form + Zod
- TanStack Query
- Socket.IO Client
- Wouter (roteamento)
- React Draggable

### Backend
- Node.js + Express
- TypeScript
- PostgreSQL (Neon)
- Drizzle ORM
- Socket.IO (WebSocket)
- JWT + bcryptjs
- Multer (uploads)
- Sharp (processamento de imagens)
- PDF-lib (thumbnails de PDF)

## 📝 APIs Disponíveis

### Autenticação
- `POST /api/auth/login` - Login
- `POST /api/auth/logout` - Logout
- `GET /api/auth/me` - Dados do usuário atual

### Contador
- `GET /api/contador` - Tempo desde data inicial + total de toques
- `POST /api/contador/touch` - Registrar toque no coração

### Cartinhas
- `GET /api/cartinhas` - Listar todas
- `POST /api/cartinhas` - Criar (com upload de PDF opcional)
- `DELETE /api/cartinhas/:id` - Remover

### Fotos
- `GET /api/photos` - Listar todas
- `POST /api/photos` - Enviar foto
- `DELETE /api/photos/:id` - Remover
- `GET /api/photos/:id/comments` - Listar comentários
- `POST /api/photos/:id/comments` - Adicionar comentário

### Post-its
- `GET /api/postits` - Listar todos
- `POST /api/postits` - Criar
- `PUT /api/postits/:id` - Atualizar (posição, conteúdo)
- `DELETE /api/postits/:id` - Remover

### Notas
- `GET /api/notes` - Listar todas
- `POST /api/notes` - Criar
- `PUT /api/notes/:id` - Atualizar
- `DELETE /api/notes/:id` - Remover

## 🔌 WebSocket (Tempo Real)

### Eventos

**Post-its**:
- `join-postits` - Entrar na sala
- `postit-created` - Post-it criado
- `postit-updated` - Post-it atualizado
- `postit-deleted` - Post-it removido

**Comentários**:
- `join-photo` - Entrar na sala de uma foto
- `new-comment` - Novo comentário

## 🎯 Próximas Melhorias (Futuras)

- Sistema de notificações push
- Versionamento de notas
- Compressão automática de imagens
- Tutorial interativo de primeira visita
- Testes de integração completos
- Service Worker para PWA
- Dashboard administrativo

## 💝 Feito com Amor

Este projeto foi criado especialmente para Jessica e Miguel celebrarem seu amor e compartilharem momentos especiais em um espaço único e personalizado.

---

**Desenvolvido em**: Novembro de 2024
**Stack**: React + TypeScript + Express + PostgreSQL + Socket.IO
