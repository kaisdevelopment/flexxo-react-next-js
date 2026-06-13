# 📘 AULA 02 — Next.js: App Router, Server Components, API Routes e Middleware

**Curso:** Desenvolvimento Front-End Moderno com React e Next.js
**Escola:** Flexxo — Polo Caxias do Sul
**Formato:** Mentoria 1:1 | **Carga horária:** 4h

---

## 🎯 Objetivos da Aula

Ao final desta aula, o aluno será capaz de:

- Entender o que é o Next.js e por que ele existe sobre o React
- Criar um projeto Next.js do zero e entender cada pasta
- Construir rotas usando apenas a estrutura de pastas (App Router)
- Diferenciar Server Components de Client Components na prática
- Criar rotas dinâmicas com parâmetros (`/servidor/[id]`)
- Construir API Routes para servir dados JSON (incluindo sub-rotas `resumo` e `offline`)
- Implementar Middleware para proteger rotas de API com chave de acesso
- Criar um componente Client que consome a API interna com autenticação
- Migrar o Dashboard da Aula 01 para dentro do Next.js

---

## 📖 Recapitulação Rápida da Aula 01

Na aula anterior, construímos um **Dashboard de Monitoramento de Servidores** usando:

| O que usamos | O que aprendemos |
|---|---|
| Vite + React | Criar projeto, rodar com `npm run dev` |
| Componentes funcionais | Header, Footer, ServerCard, etc. |
| Props | Passar dados de pai para filho |
| `useState` | Filtro por status, busca, ordenação |
| `.map()`, `.filter()`, `.sort()` | Renderizar e manipular listas |
| Renderização condicional | Alertas, mensagens vazias |

**Tudo isso continua válido* O Next.js não substitui o React — ele **envolve** o React e adiciona superpoderes.

---

## 1. O que é Next.js? (E por que não ficar só no React puro?)

### 1.1 — O Problema do React Puro

O React é incrível para construir interfaces, mas ele sozinho é **só uma biblioteca de UI**. Para uma aplicação profissional, você precisa resolver vários problemas manualmente:

| Problema | No React Puro | No Next.js |
|---|---|---|
| **Roteamento** | Instalar `react-router-dom`, configurar na mão | Automático: cria pasta = cria rota |
| **SEO** | Péssimo (HTML vazio, JS renderiza tudo) | Excelente (renderiza no servidor) |
| **Performance** | Tudo carrega de uma vez (bundle grande) | Code splitting automático por rota |
| **API Backend** | Precisa de servidor separado (Express, Laravel) | API Routes integradas no mesmo projeto |
| **Deploy** | Configurar manualmente | Um clique na Vercel |
| **Imagens** | `<img>` normal (sem otimização) | `<Image>` com lazy load, resize automático |
| **Middleware** | Não existe nativamente | `middleware.js` intercepta todas as requisições |

### 1.2 — Analogia: React é o Motor, Next.js é o Carro

Pense assim:

- **React** = Motor de um carro. Potente, essencial, mas sozinho não te leva a lugar nenhum.
- **Next.js** = O carro completo. Tem o motor (React), mas também tem direção (roteamento), carroceria (SSR/SEO), painel (DevTools), e até GPS (deploy otimizado).

Você não precisa escolher entre um e outro — **Next.js usa React por baixo**. Todo componente que você escreveu na Aula 01 funciona dentro do Next.js.

### 1.3 — Analogia para quem vem do Laravel

| Laravel | Next.js |
|---|---|
| Framework PHP full-stack | Framework React full-stack |
| `routes/web.php` | Pastas dentro de `app/` |
| `php artisan serve` | `npm run dev` |
| Blade (server-side) | Server Components (server-side) |
| Controllers retornam views | `page.jsx` retorna JSX |
| `routes/api.php` | `app/api/*/route.js` |
| Middleware (`app/Http/Middleware`) | `middleware.js` na raiz do projeto |
| `.env` | `.env.local` |

> **Se você entendeu o Laravel, vai entender o Next.js.** A filosofia é a mesma: convenção sobre configuração.

---

## 2. Criando o Projeto Next.js

### 2.1 — Setup Inicial

```bash
# Criar projeto Next.js (aceite as opções padrão)
npx create-next-app@latest next-dashboard
```

O assistente vai perguntar:

```
✔ Would you like to use TypeScript? → No
✔ Would you like to use ESLint? → Yes
✔ Would you like to use Tailwind CSS? → No  (vamos adicionar na Aula 03)
✔ Would you like to use src/ directory? → No
✔ Would you like to use App Router? → Yes  ← IMPORTANTE!
✔ Would you like to use Turbopack? → Yes
✔ Would you like to customize import alias? → No
```

```bash
cd next-dashboard
npm run dev
# Acesse: http://localhost:3000
```

### 2.2 — Estrutura de Pastas do Next.js

```
next-dashboard/
├── app/                    ← 🧠 CÉREBRO do projeto (App Router)
│   ├── layout.jsx          ← Layout raiz (equivale ao app.blade.php)
│   ├── page.jsx            ← Página inicial (rota /)
│   ├── globals.css         ← CSS global
│   └── favicon.ico
├── public/                 ← Arquivos estáticos (imagens, fontes)
├── node_modules/
├── package.json
├── next.config.mjs         ← Configurações do Next.js
├── middleware.js            ← 🛡️ Middleware (intercepta requisições)
├── .env.local              ← Variáveis de ambiente
└── jsconfig.json
```

### 2.3 — Analogia: Comparando com o Laravel

| Laravel | Next.js | Função |
|---|---|---|
| `resources/views/layouts/app.blade.php` | `app/layout.jsx` | Layout base que envolve todas as páginas |
| `resources/views/home.blade.php` | `app/page.jsx` | Página inicial |
| `public/` | `public/` | Arquivos estáticos |
| `routes/web.php` | A própria estrutura de pastas | Definição de rotas |
| `app/Http/Middleware/` | `middleware.js` na raiz | Interceptação de requisições |
| `.env` | `.env.local` | Variáveis de ambiente |

---

## 3. App Router — Pastas como Rotas

Esse é o conceito mais importante do Next.js moderno. **Não existe um arquivo de rotas.** A estrutura de pastas dentro de `app/` **é** o roteamento.

### 3.1 — Regra de Ouro

> **Toda pasta dentro de `app/` que contém um arquivo `page.jsx` se torna uma rota acessível no navegador.**

### 3.2 — Exemplos Práticos

```
app/
├── page.jsx                     → http://localhost:3000/
├── sobre/
│   └── page.jsx                 → http://localhost:3000/sobre
├── contato/
│   └── page.jsx                 → http://localhost:3000/contato
├── servidores/
│   ├── page.jsx                 → http://localhost:3000/servidores
│   └── [id]/
│       └── page.jsx             → http://localhost:3000/servidores/1
│                                  http://localhost:3000/servidores/2
│                                  http://localhost:3000/servidores/abc
└── api/
    └── servidores/
        ├── route.js             → http://localhost:3000/api/servidores
        ├── resumo/
        │   └── route.js         → http://localhost:3000/api/servidores/resumo
        ├── offline/
        │   └── route.js         → http://localhost:3000/api/servidores/offline
        └── [id]/
            └── route.js         → http://localhost:3000/api/servidores/1
```

### 3.3 — Analogia: O Prédio Comercial

Imagine que `app/` é um prédio comercial:

- Cada **pasta** é um **andar** do prédio
- O arquivo **`page.jsx`** é a **porta de entrada** daquele andar
- Se um andar não tem porta (`page.jsx`), ninguém consegue entrar (a rota não existe)
- O **`layout.jsx`** é a **estrutura do prédio** — paredes, elevador, recepção — tudo que é compartilhado entre os andares
- O **`middleware.js`** é o **segurança na portaria** — verifica se você tem crachá antes de entrar

### 3.4 — Analogia com Laravel

```php
// Laravel: routes/web.php
Route::get('/', [HomeController::class, 'index']);
Route::get('/sobre', [SobreController::class, 'index']);
Route::get('/servidores/{id}', [ServidorController::class, 'show']);
```

```
// Next.js: não precisa de arquivo de rotas!
// Basta criar as pastas:
app/page.jsx                 → equivale a Route::get('/')
app/sobre/page.jsx           → equivale a Route::get('/sobre')
app/servidores/[id]/page.jsx → equivale a Route::get('/servidores/{id}')
```

### 3.5 — Arquivos Especiais do App Router

O Next.js reconhece arquivos com nomes especiais dentro de cada pasta:

| Arquivo | Função | Analogia Laravel |
|---|---|---|
| `page.jsx` | A página/rota em si | O método do Controller que retorna a view |
| `layout.jsx` | Layout que envolve a página e todas as sub-rotas | `app.blade.php` com `@yield('content')` |
| `loading.jsx` | Exibido enquanto a página carrega | Spinner/skeleton enquanto o Blade carrega dados |
| `error.jsx` | Exibido quando ocorre um erro | `resources/views/errors/500.blade.php` |
| `not-found.jsx` | Página 404 customizada | `resources/views/errors/404.blade.php` |
| `route.js` | API endpoint (sem UI) | `routes/api.php` |
| `middleware.js` | Intercepta requisições (na raiz do projeto) | `app/Http/Middleware/` |

---

## 4. Layout — O Esqueleto Compartilhado

### 4.1 — Entendendo o `layout.jsx`

O layout é o componente que **envolve** todas as páginas. Ele renderiza uma vez e as páginas trocam **dentro** dele.

**Analogia:** No Laravel, você tem:

```html
<html>
  <body>
    @include('partials.header')
    @yield('content')   ← aqui troca a cada página
    @include('partials.footer')
  </body>
</html>
```

No Next.js, o equivalente é:

```jsx
// app/layout.jsx
export default function RootLayout({ children }) {
  return (
    <html lang="pt-BR">
      <body>
        <Header />
        {children}    {/* ← aqui troca a cada página */}
        <Footer />
      </body>
    </html>
  );
}
```

### 4.2 — Nosso Layout (Versão Prática)

Edite `app/layout.jsx`:

```jsx
import './globals.css';
import Header from '../components/Header';
import Footer from '../components/Footer';

export const metadata = {
  title: 'Server Monitor — Flexxo',
  description: 'Dashboard de Monitoramento de Servidores Linux',
};

export default function RootLayout({ children }) {
  return (
    <html lang="pt-BR">
      <body style={{
        backgroundColor: '#030712',
        color: '#F3F4F6',
        minHeight: '100vh',
        fontFamily: "'Segoe UI', sans-serif",
        display: 'flex',
        flexDirection: 'column',
        margin: 0,
      }}>
        <Header />
        <main style={{ maxWidth: '960px', margin: '0 auto', padding: '24px', width: '100%', flex: 1 }}>
          {children}
        </main>
        <Footer />
      </body>
    </html>
  );
}
```

> **`metadata`** — O Next.js usa esse objeto para gerar as tags `<title>` e `<meta>` automaticamente. Sem precisar de `<Head>` manual. Isso é SEO de graça!

---

## 5. Server Components vs Client Components

Esse é o conceito que **mais confunde** quem está começando com Next.js. Vamos desmistificar.

### 5.1 — O que são Server Components?

Por padrão, **todo componente no Next.js (App Router) é um Server Component**. Isso significa que ele:

- ✅ Roda **no servidor** (como o Blade do Laravel)
- ✅ Pode acessar banco de dados diretamente
- ✅ Pode ler arquivos do sistema
- ❌ **NÃO** pode usar `useState`, `useEffect`, `onClick`, etc.
- ❌ **NÃO** pode usar interatividade do navegador

### 5.2 — O que são Client Components?

Quando você precisa de **interatividade** (cliques, formulários, estado), você marca o componente com `"use client"` no topo:

```jsx
"use client"; // ← essa linha mágica transforma em Client Component

import { useState } from 'react';

const Contador = () => {
  const [n, setN] = useState(0);
  return <button onClick={() => setN(n + 1)}>Cliques: {n}</button>;
};
```

### 5.3 — Quando usar cada um?

| Situação | Tipo | Por quê? |
|---|---|---|
| Exibir dados estáticos (título, texto, lista) | **Server** | Não precisa de JS no navegador |
| Buscar dados do banco/API | **Server** | Acesso direto, sem expor credenciais |
| Formulário com inputs | **Client** | Precisa de `useState` para controlar valores |
| Botão que muda algo na tela | **Client** | Precisa de `onClick` e estado |
| Filtros interativos | **Client** | Precisa de estado e re-renderização |
| Layout com header/footer estáticos | **Server** | Só HTML, sem interatividade |
| Consumir API com fetch dinâmico | **Client** | Precisa de `useEffect` e `useState` |

### 5.4 — Analogia: Restaurante

Imagine um restaurante:

- **Server Components** = A **cozinha**. Prepara tudo no fundo (servidor), e manda o prato pronto (HTML) para a mesa (navegador). O cliente não vê como foi feito.
- **Client Components** = O **garçom interativo**. Está na mesa (navegador) respondendo aos pedidos em tempo real: "Quer mais sal? Troco o prato?"

### 5.5 — Analogia com Laravel

| Laravel | Next.js |
|---|---|
| Blade renderiza no servidor → HTML pronto | Server Component renderiza no servidor → HTML pronto |
| `<script>` com jQuery/JS no Blade para interatividade | Client Component com `"use client"` para interatividade |
| Controller busca dados → passa pra view | Server Component busca dados → retorna JSX |

### 5.6 — Regra Prática

> **Comece sempre como Server Component.** Só adicione `"use client"` quando o React reclamar que você está usando hooks (`useState`, `useEffect`) ou eventos (`onClick`, `onChange`).

---

## 6. Migrando o Dashboard para Next.js

Agora vem a parte prática! Vamos pegar os componentes da Aula 01 e colocar dentro do Next.js.

### 6.1 — Estrutura Alvo (Final da Aula 02)

```
next-dashboard/
├── middleware.js                ← 🛡️ Middleware de autenticação API
├── .env.local                  ← Variáveis de ambiente
├── app/
│   ├── layout.jsx              ← Layout raiz
│   ├── page.jsx                ← Página inicial (Dashboard)
│   ├── globals.css
│   ├── sobre/
│   │   └── page.jsx            ← Página /sobre
│   ├── servidor/
│   │   └── [id]/
│   │       ├── page.jsx        ← Página dinâmica /servidor/1, /servidor/2...
│   │       └── loading.jsx     ← Loading automático
│   └── api/
│       └── servidores/
│           ├── route.js        ← GET /api/servidores
│           ├── resumo/
│           │   └── route.js    ← GET /api/servidores/resumo
│           ├── offline/
│           │   └── route.js    ← GET /api/servidores/offline
│           └── [id]/
│               └── route.js    ← GET /api/servidores/:id
├── components/
│   ├── Header.jsx              ← Server Component
│   ├── Footer.jsx              ← Server Component
│   ├── ResumoCards.jsx         ← Server Component
│   ├── ServerCard.jsx          ← Server Component + Link
│   ├── StatusFilter.jsx        ← Client Component (useState)
│   ├── SearchBar.jsx           ← Client Component (onChange)
│   ├── OrderCpu.jsx            ← Client Component (onChange)
│   ├── DashboardClient.jsx     ← Client Component (orquestrador)
│   ├── PainelAPI.jsx           ← Client Component (consome API com auth)
│   └── FormServidor.jsx        ← Client Component (formulário POST)
└── data/
    └── servidores.js
```

### 6.2 — O que muda e o que NÃO muda

| Componente | Muda? | Motivo |
|---|---|---|
| `Header.jsx` | Quase nada | Sem interatividade → Server Component |
| `Footer.jsx` | Quase nada | Sem interatividade → Server Component |
| `ResumoCards.jsx` | Quase nada | Só exibe dados → Server Component |
| `ServerCard.jsx` | Adiciona Link | Vai linkar pra rota dinâmica |
| `StatusFilter.jsx` | Adiciona `"use client"` | Usa `useState` |
| `SearchBar.jsx` | Adiciona `"use client"` | Usa `onChange` |
| `OrderCpu.jsx` | Adiciona `"use client"` | Usa `onChange` |
| `App.jsx` | Substituído por `page.jsx` + `DashboardClient.jsx` | Separar server/client |
| `servidores.js` | Nada | Dados são dados |
| **`middleware.js`** | **NOVO** | Protege as API Routes com chave |
| **`PainelAPI.jsx`** | **NOVO** | Consome API com header de autenticação |
| **`FormServidor.jsx`** | **NOVO** | Formulário para adicionar servidor via POST |

---

## 7. Código Completo — Cada Arquivo

### 7.1 — Dados Mock (inalterado)

`data/servidores.js`:

```javascript
export const servidores = [
  {
    id: 1,
    nome: "srv-web-01",
    ip: "192.168.1.10",
    status: "online",
    os: "Ubuntu 24.04 LTS",
    cpu: 45,
    ram: 62,
    uptime: "45 dias",
  },
  {
    id: 2,
    nome: "srv-db-01",
    ip: "192.168.1.20",
    status: "online",
    os: "Rocky Linux 9.4",
    cpu: 72,
    ram: 87,
    uptime: "120 dias",
  },
  {
    id: 3,
    nome: "srv-backup-01",
    ip: "192.168.1.30",
    status: "offline",
    os: "Debian 12",
    cpu: 0,
    ram: 0,
    uptime: null,
  },
  {
    id: 4,
    nome: "srv-app-01",
    ip: "192.168.1.40",
    status: "warning",
    os: "Ubuntu 22.04 LTS",
    cpu: 89,
    ram: 91,
    uptime: "15 dias",
  },
  {
    id: 5,
    nome: "srv-mail-01",
    ip: "192.168.1.50",
    status: "online",
    os: "Debian 12",
    cpu: 23,
    ram: 41,
    uptime: "200 dias",
  },
  {
    id: 6,
    nome: "srv-dns-01",
    ip: "192.168.1.60",
    status: "warning",
    os: "Ubuntu 24.04 LTS",
    cpu: 78,
    ram: 55,
    uptime: "90 dias",
  },
  {
    id: 7,
    nome: "srv-monitor-01",
    ip: "192.168.1.70",
    status: "offline",
    os: "Rocky Linux 9.4",
    cpu: 0,
    ram: 0,
    uptime: null,
  },
];
```

---

### 7.2 — CSS Global

`app/globals.css`:

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #030712;
}

a {
  text-decoration: none;
  color: inherit;
}
```

---

### 7.3 — Header (Server Component)

`components/Header.jsx`:

```jsx
import Link from 'next/link';

const Header = () => (
  <header style={{
    backgroundColor: '#111827',
    borderBottom: '1px solid #1F2937',
    padding: '16px 24px',
    display: 'flex',
    justifyContent: 'space-between',
    alignItems: 'center',
  }}>
    <Link href="/">
      <h1 style={{ fontSize: '20px', fontWeight: 'bold', color: '#10B981', cursor: 'pointer' }}>
        🖥️ Server Monitor
      </h1>
    </Link>
    <nav style={{ display: 'flex', gap: '16px', alignItems: 'center' }}>
      <Link href="/" style={{ color: '#9CA3AF', fontSize: '14px' }}>Dashboard</Link>
      <Link href="/sobre" style={{ color: '#9CA3AF', fontSize: '14px' }}>Sobre</Link>
      <span style={{ fontSize: '14px', color: '#4B5563' }}>|</span>
      <span style={{ fontSize: '14px', color: '#6B7280' }}>Flexxo Infra</span>
    </nav>
  </header>
);

export default Header;
```

> **Novidade:** Usamos `Link` do Next.js em vez de `<a>`. O `Link` faz navegação sem recarregar a página (SPA navigation). É como o Turbolinks do Laravel, mas nativo.

---

### 7.4 — Footer (Server Component)

`components/Footer.jsx`:

```jsx
const Footer = () => {
  const ano = new Date().getFullYear();

  return (
    <footer style={{
      borderTop: '1px solid #1F2937',
      padding: '20px 24px',
      marginTop: '48px',
      textAlign: 'center',
      color: '#6B7280',
      fontSize: '13px',
    }}>
      <p>© {ano} Flexxo — Polo Caxias do Sul</p>
      <p style={{ marginTop: '4px', fontSize: '12px', color: '#4B5563' }}>
        Dashboard de Monitoramento v2.0 — Next.js
      </p>
    </footer>
  );
};

export default Footer;
```

---

### 7.5 — ResumoCards (Server Component)

`components/ResumoCards.jsx`:

```jsx
const CardResumo = ({ valor, label, cor }) => (
  <div style={{
    backgroundColor: `${cor}15`,
    border: `1px solid ${cor}40`,
    borderRadius: '12px',
    padding: '16px',
    textAlign: 'center',
  }}>
    <p style={{ fontSize: '32px', fontWeight: 'bold', color: cor }}>
      {valor}
    </p>
    <p style={{ fontSize: '14px', color: '#9CA3AF' }}>{label}</p>
  </div>
);

const ResumoCards = ({ servidores }) => {
  const online = servidores.filter(s => s.status === 'online').length;
  const warning = servidores.filter(s => s.status === 'warning').length;
  const offline = servidores.filter(s => s.status === 'offline').length;

  return (
    <div style={{
      display: 'grid',
      gridTemplateColumns: '1fr 1fr 1fr',
      gap: '16px',
      marginBottom: '24px',
    }}>
      <CardResumo valor={online} label="Online" cor="#10B981" />
      <CardResumo valor={warning} label="Warning" cor="#F59E0B" />
      <CardResumo valor={offline} label="Offline" cor="#EF4444" />
    </div>
  );
};

export default ResumoCards;
```

---

### 7.6 — ServerCard (Server Component + Link)

`components/ServerCard.jsx`:

```jsx
import Link from 'next/link';

const statusConfig = {
  online:  { cor: '#10B981', emoji: '🟢' },
  warning: { cor: '#F59E0B', emoji: '🟡' },
  offline: { cor: '#EF4444', emoji: '🔴' },
};

const ServerCard = ({ servidor }) => {
  const { id, nome, ip, os, status, cpu, ram, uptime } = servidor;
  const { cor, emoji } = statusConfig[status] ?? statusConfig.offline;

  return (
    <Link href={`/servidor/${id}`} style={{ textDecoration: 'none' }}>
      <div style={{
        backgroundColor: '#111827',
        border: `2px solid ${cor}40`,
        borderRadius: '12px',
        padding: '20px',
        transition: 'border-color 0.2s, transform 0.2s',
        cursor: 'pointer',
      }}>
        <div style={{
          display: 'flex',
          justifyContent: 'space-between',
          alignItems: 'start',
          marginBottom: '12px',
        }}>
          <h3 style={{ fontSize: '18px', fontWeight: 'bold', color: '#F3F4F6' }}>
            {nome}
          </h3>
          <span style={{
            backgroundColor: `${cor}20`,
            color: cor,
            padding: '4px 10px',
            borderRadius: '6px',
            fontSize: '12px',
            fontWeight: 'bold',
            textTransform: 'uppercase',
          }}>
            {emoji} {status}
          </span>
        </div>

        <div style={{ fontSize: '14px', color: '#9CA3AF', lineHeight: '1.8' }}>
          <p>📡 IP: <span style={{ color: '#D1D5DB' }}>{ip}</span></p>
          <p>💻 OS: <span style={{ color: '#D1D5DB' }}>{os}</span></p>
          <p>⚡ CPU: <span style={{ color: cpu > 80 ? '#EF4444' : '#D1D5DB' }}>{cpu}%</span></p>
          <p>🧠 RAM: <span style={{ color: ram > 85 ? '#EF4444' : '#D1D5DB' }}>{ram}%</span></p>
          <p>⏱️ Uptime: <span style={{ color: '#D1D5DB' }}>{uptime ?? 'N/A'}</span></p>
        </div>

        {(cpu > 80 || ram > 85) && (
          <p style={{
            marginTop: '12px',
            padding: '8px',
            backgroundColor: '#7F1D1D20',
            borderRadius: '6px',
            color: '#FCA5A5',
            fontSize: '13px',
          }}>
            ⚠️ Recurso(s) em estado crítico!
          </p>
        )}

        <p style={{ marginTop: '12px', fontSize: '12px', color: '#4B5563', textAlign: 'right' }}>
          Clique para ver detalhes →
        </p>
      </div>
    </Link>
  );
};

export default ServerCard;
```

> **Novidade:** Agora cada card é um link! Ao clicar, o usuário vai para `/servidor/1`, `/servidor/2`, etc.

---

### 7.7 — SearchBar (Client Component)

`components/SearchBar.jsx`:

```jsx
"use client";

const SearchBar = ({ value, onChange }) => (
  <input
    type="text"
    value={value}
    onChange={(e) => onChange(e.target.value)}
    placeholder="🔍 Buscar servidor por nome..."
    style={{
      width: '100%',
      padding: '12px 16px',
      borderRadius: '8px',
      border: '1px solid #374151',
      backgroundColor: '#111827',
      color: '#F3F4F6',
      fontSize: '14px',
      outline: 'none',
    }}
  />
);

export default SearchBar;
```

---

### 7.8 — StatusFilter (Client Component)

`components/StatusFilter.jsx`:

```jsx
"use client";

import { useState } from 'react';

const StatusFilter = ({ onFilter }) => {
  const [ativo, setAtivo] = useState('todos');
  const opcoes = ['todos', 'online', 'warning', 'offline'];

  const handleClick = (opcao) => {
    setAtivo(opcao);
    onFilter(opcao);
  };

  return (
    <div style={{ display: 'flex', gap: '8px' }}>
      {opcoes.map(opcao => (
        <button
          key={opcao}
          onClick={() => handleClick(opcao)}
          style={{
            padding: '8px 16px',
            borderRadius: '8px',
            border: 'none',
            cursor: 'pointer',
            fontWeight: '500',
            fontSize: '14px',
            backgroundColor: ativo === opcao ? '#10B981' : '#1F2937',
            color: ativo === opcao ? '#FFF' : '#9CA3AF',
            transition: 'all 0.2s',
          }}
        >
          {opcao.charAt(0).toUpperCase() + opcao.slice(1)}
        </button>
      ))}
    </div>
  );
};

export default StatusFilter;
```

---

### 7.9 — OrderCpu (Client Component)

`components/OrderCpu.jsx`:

```jsx
"use client";

const OrderCpu = ({ value, onChange }) => (
  <select
    value={value}
    onChange={(e) => onChange(e.target.value)}
    style={{
      padding: '8px',
      borderRadius: '8px',
      border: '1px solid #1F2937',
      backgroundColor: '#111827',
      color: '#F3F4F6',
      fontFamily: 'inherit',
      cursor: 'pointer',
    }}
  >
    <option value="default">Ordenar por CPU</option>
    <option value="asc">CPU ↑ Crescente</option>
    <option value="desc">CPU ↓ Decrescente</option>
  </select>
);

export default OrderCpu;
```

---

### 7.10 — DashboardClient (Client Component — Orquestrador)

Esse componente isola toda a **interatividade** (filtros, busca, ordenação) para que a `page.jsx` continue sendo Server Component.

`components/DashboardClient.jsx`:

```jsx
"use client";

import { useState } from 'react';
import ResumoCards from './ResumoCards';
import StatusFilter from './StatusFilter';
import SearchBar from './SearchBar';
import ServerCard from './ServerCard';
import OrderCpu from './OrderCpu';

const DashboardClient = ({ servidores }) => {
  const [filtro, setFiltro] = useState('todos');
  const [busca, setBusca] = useState('');
  const [ordemCpu, setOrdemCpu] = useState('default');

  const servidoresFiltrados = servidores
    .filter(s => filtro === 'todos' || s.status === filtro)
    .filter(s => s.nome.toLowerCase().includes(busca.toLowerCase()))
    .sort((a, b) => {
      if (ordemCpu === 'asc') return a.cpu - b.cpu;
      if (ordemCpu === 'desc') return b.cpu - a.cpu;
      return 0;
    });

  return (
    <>
      <ResumoCards servidores={servidores} />

      <div style={{ display: 'flex', flexDirection: 'column', gap: '16px', marginBottom: '24px' }}>
        <SearchBar value={busca} onChange={setBusca} />
        <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', flexWrap: 'wrap', gap: '12px' }}>
          <StatusFilter onFilter={setFiltro} />
          <OrderCpu value={ordemCpu} onChange={setOrdemCpu} />
          {busca && (
            <span style={{ fontSize: '14px', color: '#9CA3AF' }}>
              Encontrados: <strong>{servidoresFiltrados.length}</strong>
            </span>
          )}
        </div>
      </div>

      <div style={{
        display: 'grid',
        gridTemplateColumns: '1fr 1fr',
        gap: '16px',
      }}>
        {servidoresFiltrados.map(servidor => (
          <ServerCard key={servidor.id} servidor={servidor} />
        ))}
      </div>

      {servidoresFiltrados.length === 0 && (
        <p style={{ textAlign: 'center', color: '#6B7280', marginTop: '40px' }}>
          {busca
            ? `Nenhum servidor encontrado para \"${busca}\"`
            : `Nenhum servidor com status \"${filtro}\"`}
        </p>
      )}
    </>
  );
};

export default DashboardClient;
```

> **Padrão importante:** No Next.js, é comum criar um componente `*Client.jsx` para isolar a interatividade e manter a página (`page.jsx`) como Server Component. Esse padrão se chama **"Client Component Boundary"**.

---

### 7.11 — Página Inicial (Server Component)

`app/page.jsx`:

```jsx
import { servidores } from '../data/servidores';
import DashboardClient from '../components/DashboardClient';

export default function HomePage() {
  return (
    <>
      <h2 style={{ fontSize: '24px', fontWeight: 'bold', marginBottom: '20px' }}>
        📊 Dashboard
      </h2>
      <DashboardClient servidores={servidores} />
    </>
  );
}
```

> **Perceba:** A `page.jsx` é um **Server Component**. Ela importa os dados e passa via props para o `DashboardClient`. O servidor busca os dados, o cliente trata a interatividade. **Separação limpa de responsabilidades.**

---

## 8. Rotas Dinâmicas — Página de Detalhes do Servidor

### 8.1 — O que são Rotas Dinâmicas?

Rotas dinâmicas permitem criar uma **página para cada item** de uma lista, usando um parâmetro na URL.

**Analogia com Laravel:**

```php
// Laravel
Route::get('/servidores/{id}', [ServidorController::class, 'show']);

// No controller:
public function show($id) {
    $servidor = Servidor::findOrFail($id);
    return view('servidores.show', compact('servidor'));
}
```

```
// Next.js — é só criar a pasta com colchetes:
app/servidor/[id]/page.jsx

// O [id] vira um parâmetro, exatamente como {id} no Laravel
```

### 8.2 — Criando a Página de Detalhes

Crie a estrutura `app/servidor/[id]/page.jsx`:

```jsx
import Link from 'next/link';
import { servidores } from '../../../data/servidores';

const statusConfig = {
  online:  { cor: '#10B981', emoji: '🟢', texto: 'Operacional' },
  warning: { cor: '#F59E0B', emoji: '🟡', texto: 'Atenção Necessária' },
  offline: { cor: '#EF4444', emoji: '🔴', texto: 'Fora do Ar' },
};

export default async function ServidorPage({ params }) {
  const { id } = await params;
  const servidor = servidores.find(s => s.id === Number(id));

    return (
      <div style={{ textAlign: 'center', marginTop: '80px' }}>
        <h2 style={{ fontSize: '24px', color: '#EF4444' }}>❌ Servidor não encontrado</h2>
        <p style={{ color: '#6B7280', marginTop: '12px' }}>ID: {id}</p>
        <Link href="/" style={{ color: '#10B981', marginTop: '20px', display: 'inline-block' }}>
          ← Voltar ao Dashboard
        </Link>
      </div>
    );
  }

  const { nome, ip, os, status, cpu, ram, uptime } = servidor;
  const { cor, emoji, texto } = statusConfig[status] ?? statusConfig.offline;

  return (
    <div>
      <Link href="/" style={{ color: '#10B981', fontSize: '14px' }}>
        ← Voltar ao Dashboard
      </Link>

      <div style={{
        backgroundColor: '#111827',
        border: `2px solid ${cor}40`,
        borderRadius: '16px',
        padding: '32px',
        marginTop: '20px',
      }}>
        <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '24px' }}>
          <h1 style={{ fontSize: '28px', fontWeight: 'bold', color: '#F3F4F6' }}>
            {nome}
          </h1>
          <span style={{
            backgroundColor: `${cor}20`,
            color: cor,
            padding: '8px 16px',
            borderRadius: '8px',
            fontSize: '14px',
            fontWeight: 'bold',
            textTransform: 'uppercase',
          }}>
            {emoji} {texto}
          </span>
        </div>

        <div style={{
          display: 'grid',
          gridTemplateColumns: '1fr 1fr',
          gap: '20px',
          fontSize: '16px',
          color: '#9CA3AF',
          lineHeight: '2',
        }}>
          <div>
            <h3 style={{ color: '#F3F4F6', marginBottom: '8px', fontSize: '18px' }}>📋 Informações</h3>
            <p>📡 IP: <span style={{ color: '#D1D5DB' }}>{ip}</span></p>
            <p>💻 Sistema: <span style={{ color: '#D1D5DB' }}>{os}</span></p>
            <p>⏱️ Uptime: <span style={{ color: '#D1D5DB' }}>{uptime ?? 'N/A'}</span></p>
          </div>
          <div>
            <h3 style={{ color: '#F3F4F6', marginBottom: '8px', fontSize: '18px' }}>📊 Recursos</h3>
            <p>⚡ CPU: <span style={{ color: cpu > 80 ? '#EF4444' : '#10B981', fontWeight: 'bold' }}>{cpu}%</span></p>
            <p>🧠 RAM: <span style={{ color: ram > 85 ? '#EF4444' : '#10B981', fontWeight: 'bold' }}>{ram}%</span></p>
          </div>
        </div>

        {(cpu > 80 || ram > 85) && (
          <div style={{
            marginTop: '24px',
            padding: '16px',
            backgroundColor: '#7F1D1D20',
            borderRadius: '8px',
            border: '1px solid #7F1D1D',
            color: '#FCA5A5',
          }}>
            <p style={{ fontWeight: 'bold', marginBottom: '4px' }}>⚠️ Alerta de Recursos</p>
            <p style={{ fontSize: '14px' }}>
              {cpu > 80 && `CPU em ${cpu}% (acima de 80%). `}
              {ram > 85 && `RAM em ${ram}% (acima de 85%).`}
            </p>
          </div>
        )}
      </div>
    </div>
  );
}
```

> **Pontos importantes:**
> - `{ params }` → O Next.js injeta os parâmetros da URL automaticamente. `params.id` vem da pasta `[id]`.
> - `servidores.find(s => s.id === Number(id))` → `params.id` é sempre string, precisamos converter.
> - Esse componente é **Server Component** — não tem interatividade, só exibe dados.

---

## 9. API Routes — Criando Endpoints

### 9.1 — O que são API Routes?

API Routes permitem criar **endpoints backend** dentro do Next.js. É como ter o `routes/api.php` do Laravel dentro do seu projeto front-end!

**Analogia com Laravel:**

```php
// Laravel: routes/api.php
Route::get('/api/servidores', function () {
    return response()->json(Servidor::all());
});
```

```javascript
// Next.js: app/api/servidores/route.js
// A PASTA é a rota, o ARQUIVO route.js define os métodos HTTP
```

### 9.2 — Criando o Endpoint GET /api/servidores

Crie `app/api/servidores/route.js`:

```javascript
import { servidores } from '../../../data/servidores';

// GET /api/servidores
export async function GET(request) {
  const { searchParams } = new URL(request.url);
  const status = searchParams.get('status');
  const busca = searchParams.get('busca');

  let resultado = servidores;

  if (status && status !== 'todos') {
    resultado = resultado.filter(s => s.status === status);
  }

  if (busca) {
    resultado = resultado.filter(s =>
      s.nome.toLowerCase().includes(busca.toLowerCase())
    );
  }

  return Response.json({
    total: resultado.length,
    servidores: resultado,
  });
}

// POST /api/servidores
export async function POST(request) {
  const body = await request.json();
  const { nome, ip, os, status, cpu, ram } = body;

    return Response.json(
      { erro: 'Campos nome e ip são obrigatórios' },
      { status: 400 }
    );
  }

  const novoServidor = {
    id: servidores.length + 1,
    nome,
    ip,
    os: os || 'Linux',
    status: status || 'online',
    cpu: cpu || 0,
    ram: ram || 0,
    uptime: '0 dias',
  };

  servidores.push(novoServidor);

  return Response.json(
    { status: 201 }
  );
}
```

### 9.3 — Testando a API

Com o servidor rodando (`npm run dev`), acesse no navegador:

```
http://localhost:3000/api/servidores
http://localhost:3000/api/servidores?status=online
http://localhost:3000/api/servidores?busca=web
http://localhost:3000/api/servidores?status=warning&busca=dns
```

> **Resultado:** JSON puro, igualzinho o que o Laravel retornaria com `response()->json()`.

### 9.4 — Criando o Endpoint GET /api/servidores/[id]

Para buscar um servidor específico, crie `app/api/servidores/[id]/route.js`:

```javascript
import { servidores } from '../../../../data/servidores';

// GET /api/servidores/:id
export async function GET(request, { params }) {
  const { id } = await params;
  const servidor = servidores.find(s => s.id === Number(id));

    return Response.json(
      { error: 'Servidor não encontrado' },
      { status: 404 }
    );
  }

  return Response.json(servidor);
}
```

### 9.5 — Criando o Endpoint GET /api/servidores/resumo

Crie `app/api/servidores/resumo/route.js`:

```javascript
import { servidores } from '../../../../data/servidores';

// GET /api/servidores/resumo
export async function GET() {
  const total = servidores.length;
  const online = servidores.filter(s => s.status === 'online').length;
  const warning = servidores.filter(s => s.status === 'warning').length;
  const offline = servidores.filter(s => s.status === 'offline').length;

  const cpuMedia = Math.round(
    servidores.reduce((acc, s) => acc + s.cpu, 0) / total
  );
  const ramMedia = Math.round(
    servidores.reduce((acc, s) => acc + s.ram, 0) / total
  );

  return Response.json({
    total,
    online,
    warning,
    offline,
    cpuMedia,
    ramMedia,
  });
}
```

> **Uso real:** Esse endpoint é perfeito para alimentar cards de resumo sem precisar enviar a lista completa de servidores.

### 9.6 — Criando o Endpoint GET /api/servidores/offline

Crie `app/api/servidores/offline/route.js`:

```javascript
import { servidores } from '../../../../data/servidores';

// GET /api/servidores/offline
export async function GET() {
  const offline = servidores.filter(s => s.status === 'offline');

  return Response.json({
    total: offline.length,
    servidores: offline,
    alerta: offline.length > 0
      : '✅ Todos os servidores estão operacionais.',
  });
}
```

> **Uso real:** Esse endpoint pode alimentar um sistema de alertas ou notificações.

---

## 10. Middleware — Protegendo as API Routes

### 10.1 — O que é Middleware?

Middleware é código que **roda entre a requisição e a resposta**. Ele intercepta cada request antes de chegar na rota e pode:

- ✅ Verificar autenticação
- ✅ Validar headers
- ✅ Redirecionar
- ✅ Bloquear acesso
- ✅ Adicionar logs

### 10.2 — Analogia com Laravel

| Laravel | Next.js |
|---|---|
| `php artisan make:middleware CheckApiKey` | Criar `middleware.js` na raiz do projeto |
| Registrar em `app/Http/Kernel.php` | Exportar `config.matcher` no próprio arquivo |
| `$request->header('x-api-key')` | `request.headers.get('x-api-key')` |
| `abort(401)` | `NextResponse.json({...}, { status: 401 })` |
| `$next($request)` | `NextResponse.next()` |

### 10.3 — Analogia: O Porteiro do Prédio

Lembra da analogia do prédio comercial?

- Antes, qualquer pessoa podia entrar em qualquer andar
- Agora tem um **porteiro** (middleware) na entrada
- O porteiro pede o **crachá** (header `x-api-key`)
- O porteiro só controla as rotas que começam com `/api/` (configurado no `matcher`)

### 10.4 — Variáveis de Ambiente

Antes de criar o middleware, configure a chave de API. Crie o arquivo `.env.local` na raiz do projeto:

```bash
# .env.local
API_KEY=flexxo-2026-segura
NEXT_PUBLIC_API_KEY=flexxo-2026-segura
```

**Por que duas variáveis?**

| Variável | Onde funciona | Quem acessa |
|---|---|---|
| `API_KEY` | Apenas no servidor (middleware, API Routes) | Código backend — **SEGURA** |
| `NEXT_PUBLIC_API_KEY` | Servidor E navegador | Código client — **VISÍVEL** para o usuário |

> **Regra do Next.js:** Somente variáveis que começam com `NEXT_PUBLIC_` são expostas no navegador. As demais ficam protegidas no servidor.

> **⚠️ Em produção real:** A chave pública (`NEXT_PUBLIC_`) é apenas para desenvolvimento/demonstração. Em produção, use autenticação JWT ou OAuth.

### 10.5 — Criando o Middleware

Crie `middleware.js` na **raiz do projeto** (mesmo nível do `package.json`):

```javascript
// middleware.js
import { NextResponse } from 'next/server';

export function middleware(request) {
  console.log('🛡️ MIDDLEWARE ATIVADO:', request.nextUrl.pathname);

  // Só protege rotas que começam com /api/
  if (request.nextUrl.pathname.startsWith('/api/')) {
    const apiKey = request.headers.get('x-api-key');
    const chaveValida = process.env.API_KEY || 'flexxo-2026-segura';

      return NextResponse.json(
        {
          erro: 'Acesso negado',
          mensagem: 'Envie o header x-api-key com uma chave válida',
        },
        { status: 401 }
      );
    }

    console.log('✅ Chave válida! Acesso liberado.');
  }

  return NextResponse.next();
}

export const config = {
  matcher: '/api/:path*',
};
```

**Explicação linha a linha:**

| Código | O que faz |
|---|---|
| `import { NextResponse }` | Importa o helper de resposta do Next.js |
| `export function middleware(request)` | A função que intercepta toda requisição |
| `request.nextUrl.pathname` | A URL que está sendo acessada |
| `request.headers.get('x-api-key')` | Lê o header personalizado da requisição |
| `process.env.API_KEY` | Lê a variável de ambiente do servidor |
| `NextResponse.json({...}, { status: 401 })` | Retorna erro JSON com status 401 |
| `NextResponse.next()` | Deixa a requisição continuar (acesso liberado) |
| `config.matcher` | Define QUAIS rotas o middleware intercepta |

### 10.6 — Testando o Middleware

**❌ Sem chave (deve bloquear):**

```bash
curl http://localhost:3000/api/servidores
# Resposta: { "erro": "Acesso negado", "mensagem": "Envie o header x-api-key..." }
```

**✅ Com chave válida (deve funcionar):**

```bash
curl -H "x-api-key: flexxo-2026-segura" http://localhost:3000/api/servidores
# Resposta: { "total": 7, "servidores": [...] }
```

**❌ Com chave errada (deve bloquear):**

```bash
curl -H "x-api-key: chave-errada" http://localhost:3000/api/servidores
# Resposta: { "erro": "Acesso negado", ... }
```

> **Dica debug:** Olhe o terminal onde o `npm run dev` está rodando. Você deve ver:
> - `🛡️ MIDDLEWARE ATIVADO: /api/servidores`
> - `✅ Chave válida! Acesso liberado.` (se a chave estiver correta)

### 10.7 — Troubleshooting do Middleware

Se o middleware **não está funcionando**, verifique:

| Problema | Solução |
|---|---|
| Middleware não intercepta nada | O arquivo deve estar na **raiz** do projeto (ao lado do `package.json`), NÃO dentro de `app/` |
| Nome errado | Deve ser `middleware.js` (singular, minúsculo) |
| Não aparece log no terminal | Reinicie o servidor (`npm run dev`) |
| Chave não bate | Verifique `.env.local` e reinicie o servidor |
| Rotas de página bloqueadas | Confirme que o `matcher` só pega `/api/:path*` |

---

## 11. Consumindo a API com Autenticação — PainelAPI

Agora que a API está protegida, precisamos de um componente que envie o header `x-api-key` ao consumir os endpoints.

### 11.1 — O Componente PainelAPI

`components/PainelAPI.jsx`:

```jsx
"use client";

import { useState, useEffect } from 'react';
import FormServidor from './FormServidor';

export default function PainelAPI() {
  const [resumo, setResumo] = useState(null);
  const [offline, setOffline] = useState(null);
  const [erro, setErro] = useState(null);
  const [carregando, setCarregando] = useState(true);

  const buscarDados = async () => {
    setCarregando(true);
    setErro(null);
    try {
      const headers = {
        'x-api-key': process.env.NEXT_PUBLIC_API_KEY || 'flexxo-2026-segura',
      };

      const [resResumo, resOffline] = await Promise.all([
        fetch('/api/servidores/resumo', { headers }),
        fetch('/api/servidores/offline', { headers }),
      ]);

      if (resResumo.status === 401 || resOffline.status === 401) {
        throw new Error('🔒 Acesso negado! Verifique a chave de API.');
      }

        throw new Error('Erro ao buscar dados da API');
      }

      const dadosResumo = await resResumo.json();
      const dadosOffline = await resOffline.json();

      setResumo(dadosResumo);
      setOffline(dadosOffline);
    } catch (err) {
      console.error('Erro:', err.message);
      setErro(err.message);
    } finally {
      setCarregando(false);
    }
  };

  useEffect(() => { buscarDados(); }, []);

  const cardStyle = {
    backgroundColor: '#111827',
    border: '1px solid #1F2937',
    borderRadius: '12px',
    padding: '20px',
  };

  if (carregando) {
    return (
      <div style={{ textAlign: 'center', padding: '40px', color: '#9CA3AF' }}>
        <p style={{ fontSize: '24px' }}>⏳</p>
        <p>Carregando dados da API...</p>
      </div>
    );
  }

  if (erro) {
    return (
      <div style={{
        padding: '20px',
        backgroundColor: '#7F1D1D20',
        border: '1px solid #7F1D1D',
        borderRadius: '12px',
        color: '#FCA5A5',
        textAlign: 'center',
      }}>
        <p style={{ fontSize: '18px', fontWeight: 'bold' }}>❌ Erro na API</p>
        <p style={{ marginTop: '8px' }}>{erro}</p>
        <button
          onClick={buscarDados}
          style={{
            marginTop: '16px',
            padding: '8px 24px',
            backgroundColor: '#10B981',
            color: '#FFF',
            border: 'none',
            borderRadius: '8px',
            cursor: 'pointer',
          }}
        >
          🔄 Tentar novamente
        </button>
      </div>
    );
  }

  return (
    <div style={{ display: 'flex', flexDirection: 'column', gap: '20px' }}>
      <h3 style={{ color: '#F3F4F6', fontSize: '18px' }}>📡 Dados via API (com autenticação)</h3>

      {/* Cards de Resumo */}
      {resumo && (
        <div style={{ display: 'grid', gridTemplateColumns: 'repeat(3, 1fr)', gap: '12px' }}>
          <div style={{ ...cardStyle, textAlign: 'center' }}>
            <p style={{ fontSize: '28px', fontWeight: 'bold', color: '#10B981' }}>{resumo.online}</p>
            <p style={{ color: '#9CA3AF', fontSize: '13px' }}>Online</p>
          </div>
          <div style={{ ...cardStyle, textAlign: 'center' }}>
            <p style={{ fontSize: '28px', fontWeight: 'bold', color: '#F59E0B' }}>{resumo.warning}</p>
            <p style={{ color: '#9CA3AF', fontSize: '13px' }}>Warning</p>
          </div>
          <div style={{ ...cardStyle, textAlign: 'center' }}>
            <p style={{ fontSize: '28px', fontWeight: 'bold', color: '#EF4444' }}>{resumo.offline}</p>
            <p style={{ color: '#9CA3AF', fontSize: '13px' }}>Offline</p>
          </div>
        </div>
      )}

      {/* Médias */}
      {resumo && (
        <div style={cardStyle}>
          <p style={{ color: '#9CA3AF' }}>
            📊 CPU Média: <strong style={{ color: resumo.cpuMedia > 70 ? '#EF4444' : '#10B981' }}>{resumo.cpuMedia}%</strong>
            &nbsp;&nbsp;|&nbsp;&nbsp;
            🧠 RAM Média: <strong style={{ color: resumo.ramMedia > 70 ? '#EF4444' : '#10B981' }}>{resumo.ramMedia}%</strong>
            &nbsp;&nbsp;|&nbsp;&nbsp;
            🖥️ Total: <strong style={{ color: '#F3F4F6' }}>{resumo.total}</strong>
          </p>
        </div>
      )}

      {/* Alerta de Servidores Offline */}
      {offline && (
        <div style={{
          ...cardStyle,
          borderColor: offline.total > 0 ? '#7F1D1D' : '#065F4640',
          backgroundColor: offline.total > 0 ? '#7F1D1D20' : '#065F4620',
        }}>
          <p style={{
            color: offline.total > 0 ? '#FCA5A5' : '#6EE7B7',
            fontWeight: 'bold',
          }}>
            {offline.alerta}
          </p>
          {offline.total > 0 && (
            <ul style={{ marginTop: '8px', paddingLeft: '20px', color: '#FCA5A5', fontSize: '14px' }}>
              {offline.servidores.map(s => (
                <li key={s.id}>🔴 {s.nome} ({s.ip}) — {s.os}</li>
              ))}
            </ul>
          )}
        </div>
      )}

      {/* Formulário para adicionar servidor */}
      <FormServidor onServidorAdicionado={buscarDados} />

      {/* Botão de atualizar */}
      <button
        onClick={buscarDados}
        style={{
          padding: '10px 24px',
          backgroundColor: '#1F2937',
          color: '#9CA3AF',
          border: '1px solid #374151',
          borderRadius: '8px',
          cursor: 'pointer',
          alignSelf: 'flex-start',
        }}
      >
        🔄 Atualizar dados
      </button>
    </div>
  );
}
```

### 11.2 — O Componente FormServidor

`components/FormServidor.jsx`:

```jsx
"use client";

import { useState } from 'react';

export default function FormServidor({ onServidorAdicionado }) {
  const [nome, setNome] = useState('');
  const [ip, setIp] = useState('');
  const [os, setOs] = useState('Ubuntu 24.04 LTS');
  const [mensagem, setMensagem] = useState(null);
  const [enviando, setEnviando] = useState(false);

  const handleSubmit = async (e) => {
    e.preventDefault();
    setEnviando(true);
    setMensagem(null);

    try {
      const res = await fetch('/api/servidores', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'x-api-key': process.env.NEXT_PUBLIC_API_KEY || 'flexxo-2026-segura',
        },
        body: JSON.stringify({ nome, ip, os, status: 'online', cpu: 0, ram: 0 }),
      });

      const dados = await res.json();

        throw new Error(dados.erro || 'Erro ao adicionar servidor');
      }

      setMensagem({ tipo: 'sucesso', texto: `✅ ${dados.mensagem} (ID: ${dados.servidor.id})` });
      setNome('');
      setIp('');

      if (onServidorAdicionado) onServidorAdicionado();
    } catch (err) {
      setMensagem({ tipo: 'erro', texto: `❌ ${err.message}` });
    } finally {
      setEnviando(false);
    }
  };

  const inputStyle = {
    padding: '10px 14px',
    borderRadius: '8px',
    border: '1px solid #374151',
    backgroundColor: '#1F2937',
    color: '#F3F4F6',
    fontSize: '14px',
    outline: 'none',
    width: '100%',
  };

  return (
    <div style={{
      backgroundColor: '#111827',
      border: '1px solid #1F2937',
      borderRadius: '12px',
      padding: '20px',
    }}>
      <h4 style={{ color: '#F3F4F6', marginBottom: '16px' }}>➕ Adicionar Servidor (POST)</h4>

      <form onSubmit={handleSubmit} style={{ display: 'flex', flexDirection: 'column', gap: '12px' }}>
        <input
          type="text"
          placeholder="Nome do servidor (ex: srv-novo-01)"
          value={nome}
          onChange={(e) => setNome(e.target.value)}
          style={inputStyle}
          required
        />
        <input
          type="text"
          placeholder="IP (ex: 192.168.1.100)"
          value={ip}
          onChange={(e) => setIp(e.target.value)}
          style={inputStyle}
          required
        />
        <select
          value={os}
          onChange={(e) => setOs(e.target.value)}
          style={{ ...inputStyle, cursor: 'pointer' }}
        >
          <option>Ubuntu 24.04 LTS</option>
          <option>Ubuntu 22.04 LTS</option>
          <option>Debian 12</option>
          <option>Rocky Linux 9.4</option>
        </select>
        <button
          type="submit"
          disabled={enviando}
          style={{
            padding: '10px',
            backgroundColor: enviando ? '#374151' : '#10B981',
            color: '#FFF',
            border: 'none',
            borderRadius: '8px',
            cursor: enviando ? 'not-allowed' : 'pointer',
            fontWeight: 'bold',
          }}
        >
          {enviando ? '⏳ Enviando...' : '🚀 Adicionar Servidor'}
        </button>
      </form>

      {mensagem && (
        <p style={{
          marginTop: '12px',
          padding: '10px',
          borderRadius: '8px',
          backgroundColor: mensagem.tipo === 'sucesso' ? '#065F4620' : '#7F1D1D20',
          color: mensagem.tipo === 'sucesso' ? '#6EE7B7' : '#FCA5A5',
          fontSize: '14px',
        }}>
          {mensagem.texto}
        </p>
      )}
    </div>
  );
}
```

### 11.3 — Integrando o PainelAPI na Página Inicial

Atualize `app/page.jsx` para incluir o painel:

```jsx
import { servidores } from '../data/servidores';
import DashboardClient from '../components/DashboardClient';
import PainelAPI from '../components/PainelAPI';

export default function HomePage() {
  return (
    <>
      <h2 style={{ fontSize: '24px', fontWeight: 'bold', marginBottom: '20px' }}>
        📊 Dashboard
      </h2>
      <DashboardClient servidores={servidores} />

      <div style={{ marginTop: '48px', borderTop: '1px solid #1F2937', paddingTop: '32px' }}>
        <PainelAPI />
      </div>
    </>
  );
}
```

---

## 12. Página Sobre (Rota Estática Simples)

Para demonstrar que criar rotas é literalmente criar pastas, vamos adicionar uma página `/sobre`.

Crie `app/sobre/page.jsx`:

```jsx
import Link from 'next/link';

export default function SobrePage() {
  return (
    <div style={{ maxWidth: '600px' }}>
      <Link href="/" style={{ color: '#10B981', fontSize: '14px' }}>
        ← Voltar ao Dashboard
      </Link>

      <h1 style={{ fontSize: '28px', fontWeight: 'bold', marginTop: '20px', marginBottom: '16px' }}>
        📖 Sobre o Projeto
      </h1>

      <div style={{ color: '#9CA3AF', lineHeight: '1.8', fontSize: '15px' }}>
        <p style={{ marginBottom: '12px' }}>
          O <strong style={{ color: '#F3F4F6' }}>Server Monitor</strong> é um dashboard
          de monitoramento de servidores Linux, desenvolvido como projeto didático
          no curso de <strong style={{ color: '#10B981' }}>React + Next.js</strong> da
          escola Flexxo.
        </p>

        <h2 style={{ color: '#F3F4F6', fontSize: '18px', marginTop: '24px', marginBottom: '12px' }}>
          🛠️ Tecnologias
        </h2>
        <ul style={{ paddingLeft: '20px' }}>
          <li>React 18 — Componentes funcionais, hooks</li>
          <li>Next.js 14+ — App Router, Server Components, Middleware</li>
          <li>JavaScript ES6+ — Moderno e limpo</li>
        </ul>

        <h2 style={{ color: '#F3F4F6', fontSize: '18px', marginTop: '24px', marginBottom: '12px' }}>
          🎯 Conceitos Aplicados
        </h2>
        <ul style={{ paddingLeft: '20px' }}>
          <li>Server Components vs Client Components</li>
          <li>App Router com rotas dinâmicas</li>
          <li>API Routes integradas (GET + POST)</li>
          <li>Middleware com autenticação por header</li>
          <li>Filtros, busca e ordenação interativa</li>
          <li>Variáveis de ambiente (.env.local)</li>
        </ul>
      </div>
    </div>
  );
}
```

---

## 13. Loading States — Feedback Visual

### 13.1 — O Arquivo Mágico `loading.jsx`

O Next.js tem um recurso incrível: se você criar um arquivo `loading.jsx` dentro de uma pasta de rota, ele é exibido **automaticamente** enquanto a página carrega.

Crie `app/servidor/[id]/loading.jsx`:

```jsx
export default function Loading() {
  return (
    <div style={{ textAlign: 'center', marginTop: '80px' }}>
      <p style={{ fontSize: '24px' }}>⏳</p>
      <p style={{ color: '#9CA3AF', marginTop: '12px' }}>Carregando dados do servidor...</p>
    </div>
  );
}
```

> **Analogia:** No Laravel, quando uma query pesada roda, o usuário vê uma tela branca. No Next.js, o `loading.jsx` resolve isso automaticamente com **React Suspense** por baixo dos panos.

---

## 14. Navegação — Link vs `<a>`

### 14.1 — Por que usar `<Link>` do Next.js?

| `<a href="...">` | `<Link href="...">` |
|---|---|
| Recarrega a página inteira | Navega sem recarregar (SPA) |
| Perde o estado dos componentes | Mantém o estado |
| Lento (download completo) | Rápido (só troca o conteúdo) |
| HTML tradicional | Pré-carrega a rota (prefetch) |

```jsx
// ❌ Evite
<a href="/sobre">Sobre</a>

// ✅ Use
import Link from 'next/link';
<Link href="/sobre">Sobre</Link>
```

> **Analogia Laravel:** O `<Link>` funciona como o **Turbolinks** (ou **Inertia.js**) — a navegação acontece sem recarregar o layout, só trocando o conteúdo.

---

## 15. Conceitos Aplicados — Resumo Visual

| Conceito Next.js | Onde aparece no projeto |
|---|---|
| App Router | Toda a estrutura de pastas `app/` |
| `page.jsx` | Página inicial, sobre, detalhes do servidor |
| `layout.jsx` | Layout raiz com Header e Footer |
| Server Component | Header, Footer, ResumoCards, ServerCard, page.jsx |
| Client Component (`"use client"`) | DashboardClient, StatusFilter, SearchBar, OrderCpu, PainelAPI, FormServidor |
| Rotas dinâmicas (`[id]`) | `app/servidor/[id]/page.jsx` |
| API Routes | `app/api/servidores/route.js`, `resumo/route.js`, `offline/route.js` |
| API POST | `POST /api/servidores` — adicionar servidor |
| Middleware | `middleware.js` protegendo `/api/*` |
| Variáveis de ambiente | `.env.local` com `API_KEY` e `NEXT_PUBLIC_API_KEY` |
| `Link` do Next.js | Navegação no Header e nos cards |
| `loading.jsx` | Loading da página de detalhes |
| `metadata` | SEO no layout.jsx |
| Query params na API | `?status=online&busca=web` |
| `fetch` com headers | PainelAPI e FormServidor enviam `x-api-key` |
| `Promise.all` | PainelAPI busca resumo + offline em paralelo |

---

## 16. Exercícios Práticos

### Exercício 1 — Criar endpoint `/api/servidores/online`

**Objetivo:** Criar um endpoint que retorne apenas os servidores com status `online`.

**Instruções:**
1. Crie a pasta `app/api/servidores/online/`
2. Crie o arquivo `route.js` dentro dela
3. O retorno deve ter o formato:

```json
{
  "total": 3,
  "servidores": [...]
}
```

**Dica:** Siga o mesmo padrão do endpoint `/api/servidores/offline`.

---

### Exercício 2 — Criar endpoint `/api/servidores/criticos`

**Objetivo:** Retornar servidores com CPU > 80% OU RAM > 85%.

**Instruções:**
1. Crie `app/api/servidores/criticos/route.js`
2. Filtre os servidores que estão em estado crítico
3. Retorno esperado:

```json
{
  "total": 1,
  "servidores": [{ "nome": "srv-app-01", ... }],
}
```

---

### Exercício 3 — Exibir servidores críticos no PainelAPI

**Objetivo:** Adicionar ao componente `PainelAPI.jsx` uma seção que busca e exibe dados do endpoint `/api/servidores/criticos`.

**Instruções:**
1. Adicione um novo `useState` para `criticos`
2. Inclua o fetch no `Promise.all` junto com resumo e offline
3. Renderize um card com fundo amarelo/vermelho listando os servidores críticos

---

### Exercício 4 — Adicionar rota `/servidores` (lista completa)

**Objetivo:** Criar uma nova página em `app/servidores/page.jsx` que consuma o endpoint `GET /api/servidores` e exiba todos os servidores em formato de tabela (sem filtros interativos).

**Instruções:**
1. Crie `app/servidores/page.jsx`
2. Marque como `"use client"` (vai usar `useEffect` e `useState`)
3. Faça fetch com header `x-api-key`
4. Renderize uma tabela HTML simples com colunas: Nome, IP, Status, CPU, RAM
5. Adicione link no Header para essa nova página

---

### Exercício 5 — Middleware com rate limiting simples

**Objetivo:** Adicionar no middleware um contador que bloqueia após 10 requisições por minuto (simulação básica de rate limiting).

**Dica:** Use um `Map()` no escopo do módulo para armazenar contadores por IP. Em produção real, use Redis ou similar.

---

## 17. Checklist de Conceitos da Aula 02

| # | Conceito | Status |
|---|---|---|
| 1 | O que é Next.js e por que usar | ⬜ |
| 2 | Criar projeto com `create-next-app` | ⬜ |
| 3 | Estrutura de pastas do App Router | ⬜ |
| 4 | `page.jsx` = rota | ⬜ |
| 5 | `layout.jsx` = layout compartilhado | ⬜ |
| 6 | Server Components (padrão, sem hooks) | ⬜ |
| 7 | Client Components (`"use client"`, com hooks) | ⬜ |
| 8 | Quando usar Server vs Client | ⬜ |
| 9 | Rotas dinâmicas com `[id]` | ⬜ |
| 10 | `params` — receber parâmetros da URL | ⬜ |
| 11 | API Routes com `route.js` (GET) | ⬜ |
| 12 | API Routes com `route.js` (POST) | ⬜ |
| 13 | Sub-rotas de API (`resumo`, `offline`) | ⬜ |
| 14 | Query params na API (`searchParams`) | ⬜ |
| 15 | Middleware — proteção de rotas | ⬜ |
| 16 | Variáveis de ambiente (`.env.local`) | ⬜ |
| 17 | `NEXT_PUBLIC_` vs variáveis privadas | ⬜ |
| 18 | Consumir API com `fetch` + headers | ⬜ |
| 19 | `Promise.all` para requests paralelos | ⬜ |
| 20 | `Link` do Next.js vs `<a>` | ⬜ |
| 21 | `loading.jsx` — loading automático | ⬜ |
| 22 | `metadata` — SEO automático | ⬜ |
| 23 | Padrão Client Component Boundary | ⬜ |
| 24 | Formulário com POST para API | ⬜ |
| 25 | Dashboard migrado e funcionando no Next.js | ⬜ |

---

## 🔭 Próxima Aula

**Aula 03 — Estilização com Tailwind CSS, Integração com API Externa e Deploy**

Na última aula, vamos:

- Instalar e configurar **Tailwind CSS** no Next.js
- Refatorar toda a estilização inline para classes Tailwind
- Usar **`useEffect`** avançado com cleanup e dependências
- Integrar com uma **API REST externa** (ex: Laravel)
- Trabalhar com **autenticação JWT** (login → token → requisições)
- Fazer o **build** (`npm run build`) e entender a saída
- Fazer o **deploy na Vercel** com um clique
- Entregar o **projeto final completo**

---

## 📚 Referências

- [Next.js — Documentação Oficial](https://nextjs.org/docs)
- [Next.js — App Router](https://nextjs.org/docs/app)
- [Next.js — Middleware](https://nextjs.org/docs/app/building-your-application/routing/middleware)
- [Next.js — Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)
- [Next.js — Environment Variables](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables)
- [React — Server Components](https://react.dev/reference/rsc/server-components)
- [Next.js — Link Component](https://nextjs.org/docs/app/api-reference/components/link)

---

*Material didático — Escola Flexxo, Polo Caxias do Sul © 2026*
