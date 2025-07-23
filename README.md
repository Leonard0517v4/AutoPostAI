# 🚀 AutoPostAI

Gere conteúdos prontos e criativos para redes sociais com IA, em segundos.

## ✨ Funcionalidades
- Geração de conteúdo com OpenAI (GPT-4o)
- Autenticação via Google (Supabase)
- Limite de 5 gerações por usuário por dia
- Histórico salvo automaticamente no banco
- Interface responsiva com TailwindCSS
import { useState } from 'react'
import AuthButtons from '../components/AuthButtons'
import { supabase } from '../lib/supabaseClient'

export default function Home() {
  const [form, setForm] = useState({
    tema: '',
    plataforma: '',
    formato: '',
    objetivo: '',
    tom: '',
    publico: ''
  })

  const [gerando, setGerando] = useState(false)
  const [resultado, setResultado] = useState('')

  const handleChange = (e) => {
    setForm({ ...form, [e.target.name]: e.target.value })
  }

  const handleSubmit = async (e) => {
    e.preventDefault()
    setGerando(true)
    setResultado('')

    const session = await supabase.auth.getSession()
    const token = session.data.session?.access_token

    try {
      const res = await fetch('/api/gerar', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${token}`
        },
        body: JSON.stringify(form)
      })

      const data = await res.json()
      if (res.ok) {
        setResultado(data.conteudo)
      } else {
        setResultado('Erro: ' + data.error)
      }
    } catch (err) {
      setResultado('Erro ao conectar com o servidor.')
    } finally {
      setGerando(false)
    }
  }

  return (
    <main className="min-h-screen bg-gray-100 p-6">
      <div className="max-w-2xl mx-auto bg-white p-8 rounded shadow">
        <AuthButtons />

        <h1 className="text-2xl font-bold mb-4 text-center">AutoPostAI 🚀</h1>

        <form onSubmit={handleSubmit} className="space-y-4">
          {['tema', 'plataforma', 'formato', 'objetivo', 'tom', 'publico'].map((field) => (
            <input
              key={field}
              name={field}
              placeholder={field.charAt(0).toUpperCase() + field.slice(1)}
              value={form[field]}
              onChange={handleChange}
              className="w-full px-4 py-2 border border-gray-300 rounded"
              required
            />
          ))}

          <button
            type="submit"
            className="w-full bg-blue-600 text-white py-2 rounded hover:bg-blue-700 transition"
            disabled={gerando}
          >
            {gerando ? 'Gerando...' : 'Gerar Conteúdo'}
          </button>
        </form>

        {resultado && (
          <div className="mt-6 bg-gray-50 p-4 rounded border border-gray-200 whitespace-pre-line">
            <h2 className="font-semibold mb-2">Conteúdo Gerado:</h2>
            <p>{resultado}</p>
          </div>
        )}
      </div>
    </main>
  )
}

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
