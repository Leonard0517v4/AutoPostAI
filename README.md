# 🚀 AutoPostAI

Gere conteúdos prontos e criativos para redes sociais com IA, em segundos.

## ✨ Funcionalidades
- Geração de conteúdo com OpenAI (GPT-4o)
- Autenticação via Google (Supabase)
- Limite de 5 gerações por usuário por dia
- Histórico salvo automaticamente no banco
- Interface responsiva com TailwindCSS

## 📦 Instalação
1. Clone o repositório:
```bash
git clone https://github.com/seu-usuario/AutoPostAI.git
cd AutoPostAI
npm install
```

2. Crie o arquivo `.env.local`:
```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
OPENAI_API_KEY=
```

3. Rode localmente:
```bash
npm run dev
```

## 🛠️ Supabase
Tabela: `historico`
```sql
create table historico (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references auth.users(id),
  tema text,
  plataforma text,
  formato text,
  objetivo text,
  tom text,
  publico text,
  conteudo text,
  criado_em timestamp with time zone default now()
);
```

Feito com ❤️ por Leonardo Silva
