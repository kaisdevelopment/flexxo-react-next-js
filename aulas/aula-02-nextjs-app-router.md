# 📘 AULA 02 — Next.js: App Router, Server Components e Rotas Dinâmicas

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
- Construir API Routes para servir dados JSON
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
| Middleware | `middleware.js` no Next.js |

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
└── jsconfig.json
```

### 2.3 — Analogia: Comparando com o Laravel

| Laravel | Next.js | Função |
|---|---|---|
| `resources/views/layouts/app.blade.php` | `app/layout.jsx` | Layout base que envolve todas as páginas |
| `resources/views/home.blade.php` | `app/page.jsx` | Página inicial |
| `public/` | `public/` | Arquivos estáticos |
| `routes/web.php` | A própria estrutura de pastas | Definição de rotas |
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
        └── route.js             → http://localhost:3000/api/servidores
```

### 3.3 — Analogia: O Prédio Comercial

Imagine que `app/` é um prédio comercial:

- Cada **pasta** é um **andar** do prédio
- O arquivo **`page.jsx`** é a **porta de entrada** daquele andar
- Se um andar não tem porta (`page.jsx`), ninguém consegue entrar (a rota não existe)
- O **`layout.jsx`** é a **estrutura do prédio** — paredes, elevador, recepção — tudo que é compartilhado entre os andares

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

### 6.1 — Estrutura Alvo

```
next-dashboard/
├── app/
│   ├── layout.jsx              ← Layout raiz (já fizemos acima)
│   ├── page.jsx                ← Página inicial (Dashboard)
│   ├── globals.css
│   ├── sobre/
│   │   └── page.jsx            ← Página /sobre
│   ├── servidor/
│   │   └── [id]/
│   │       └── page.jsx        ← Página dinâmica /servidor/1, /servidor/2...
│   └── api/
│       └── servidores/
│           └── route.js        ← API: GET /api/servidores
├── components/
│   ├── Header.jsx              ← Server Component (sem interatividade)
│   ├── Footer.jsx              ← Server Component
│   ├── ResumoCards.jsx          ← Server Component
│   ├── ServerCard.jsx           ← Server Component
│   ├── StatusFilter.jsx         ← Client Component (tem useState)
│   ├── SearchBar.jsx            ← Client Component (tem onChange)
│   ├── OrderCpu.jsx             ← Client Component (tem onChange)
│   └── DashboardClient.jsx      ← Client Component (orquestra os filtros)
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

> **Novidade:** Agora cada card é um link! Ao clicar, o usuário vai para `/servidor/1`, `/servidor/2`, etc. — isso é a **rota dinâmica** que vamos criar.

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

Esse é um componente **novo**. Na Aula 01, a lógica de filtro/busca/ordenação estava no `App.jsx`. Como essa lógica usa `useState`, ela precisa ser um Client Component. Mas queremos que a **página** (`page.jsx`) seja Server Component. Solução? Separar!

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
  // Pegar query params (ex: /api/servidores?status=online)
  const { searchParams } = new URL(request.url);
  const status = searchParams.get('status');
  const busca = searchParams.get('busca');

  let resultado = servidores;

  // Filtrar por status
  if (status && status !== 'todos') {
    resultado = resultado.filter(s => s.status === status);
  }

  // Filtrar por busca
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

Teste:

```
http://localhost:3000/api/servidores/1   → retorna srv-web-01
http://localhost:3000/api/servidores/99  → retorna { error: '...' } com status 404
```

---

## 10. Página Sobre (Rota Estática Simples)

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
          <li>Next.js 14+ — App Router, Server Components</li>
          <li>JavaScript ES6+ — Moderno e limpo</li>
        </ul>

        <h2 style={{ color: '#F3F4F6', fontSize: '18px', marginTop: '24px', marginBottom: '12px' }}>
          🎯 Conceitos Aplicados
        </h2>
        <ul style={{ paddingLeft: '20px' }}>
          <li>Server Components vs Client Components</li>
          <li>App Router com rotas dinâmicas</li>
          <li>API Routes integradas</li>
          <li>Filtros, busca e ordenação interativa</li>
        </ul>
      </div>
    </div>
  );
}
```

> Pronto! Criou a pasta `sobre/` com `page.jsx` → a rota `/sobre` já existe. Sem configurar nada. **Isso é o App Router.**

---

## 11. Loading States — Feedback Visual

### 11.1 — O Arquivo Mágico `loading.jsx`

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

## 12. Navegação — Link vs `<a>`

### 12.1 — Por que usar `<Link>` do Next.js?

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

## 13. Conceitos Aplicados — Resumo Visual

| Conceito Next.js | Onde aparece no projeto |
|---|---|
| App Router | Toda a estrutura de pastas `app/` |
| `page.jsx` | Página inicial, sobre, detalhes do servidor |
| `layout.jsx` | Layout raiz com Header e Footer |
| Server Component | Header, Footer, ResumoCards, ServerCard, page.jsx |
| Client Component (`"use client"`) | DashboardClient, StatusFilter, SearchBar, OrderCpu |
| Rotas dinâmicas (`[id]`) | `app/servidor/[id]/page.jsx` |
| API Routes | `app/api/servidores/route.js` |
| `Link` do Next.js | Navegação no Header e nos cards |
| `loading.jsx` | Loading da página de detalhes |
| `metadata` | SEO no layout.jsx |
| Query params na API | `?status=online&busca=web` |

---

## 14. Checklist de Conceitos da Aula 02

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
| 11 | API Routes com `route.js` | ⬜ |
| 12 | Query params na API (`searchParams`) | ⬜ |
| 13 | `Link` do Next.js vs `<a>` | ⬜ |
| 14 | `loading.jsx` — loading automático | ⬜ |
| 15 | `metadata` — SEO automático | ⬜ |
| 16 | Padrão Client Component Boundary | ⬜ |
| 17 | Dashboard migrado e funcionando no Next.js | ⬜ |

---

## 🔭 Próxima Aula

**Aula 03 — Estilização, Integração com API e Deploy**

Na última aula, vamos:

- Usar **`useEffect`** para fetch de dados da nossa API Route
- Criar formulários com validação
- Integrar com uma **API REST externa** (ex: Laravel)
- Fazer o **deploy na Vercel** com um clique
- Entregar o **projeto final completo**

---

## 📚 Referências

- [Next.js — Documentação Oficial](https://nextjs.org/docs)
- [Next.js — App Router](https://nextjs.org/docs/app)
- [React — Server Components](https://react.dev/reference/rsc/server-components)
- [Next.js — API Routes](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)
- [Next.js — Link Component](https://nextjs.org/docs/app/api-reference/components/link)

---

*Material didático — Escola Flexxo, Polo Caxias do Sul © 2026*
