# ⚛️ Curso: Desenvolvimento Front-End Moderno com React e Next.js

> Projeto didático desenvolvido durante o curso de **React + Next.js** na escola **Flexxo** (Caxias do Sul/RS).
> Formato mentoria 1:1, focado em aplicabilidade real no mercado corporativo.

---

## 📚 Sobre o Curso

| Item              | Detalhe                                      |
|-------------------|----------------------------------------------|
| **Escola**        | Flexxo — Polo Caxias do Sul                  |
| **Formato**       | Mentoria 1:1                                 |
| **Carga horária** | 12 horas (3 aulas de 4h)                     |
| **Tecnologias**   | JavaScript ES6+, React 18, Next.js 14+       |
| **Node.js**       | 18+ (LTS)                                    |
| **Ferramentas**   | Vite, npm, VS Code                           |

---

## 🎯 Objetivo Geral

Capacitar o aluno a construir aplicações front-end modernas e profissionais utilizando React e Next.js, partindo dos fundamentos do JavaScript moderno até o deploy de uma aplicação full-stack.

---

## 🗓️ Planejamento das Aulas

### Aula 01 — JavaScript Moderno (ES6+) e Introdução ao React

| # | Tópico | Duração |
|---|--------|---------|
| 1 | JavaScript ES6+: `const/let`, arrow functions, template literals | 45min |
| 2 | Destructuring, spread/rest, operadores modernos (`??`, `?.`) | 45min |
| 3 | Métodos de array: `.map()`, `.filter()`, `.find()`, `.reduce()` | 30min |
| 4 | Async/Await e Modules (import/export) | 20min |
| 5 | O que é React, JSX e Componentes Funcionais | 30min |
| 6 | Props, Estado (`useState`) e Renderização Condicional | 30min |
| 7 | **Projeto Prático:** Dashboard de Servidores | 40min |

📄 [Material completo da Aula 01](./aulas/aula-01-js-moderno-react.md)

---

### Aula 02 — Next.js: App Router, Rotas e Server Components

| # | Tópico | Duração |
|---|--------|---------|
| 1 | O que é Next.js e por que usar sobre o React puro | 20min |
| 2 | App Router: estrutura de pastas como rotas | 40min |
| 3 | Server Components vs Client Components | 40min |
| 4 | Rotas dinâmicas e parâmetros | 30min |
| 5 | API Routes: criando endpoints no Next.js | 30min |
| 6 | Fetch de dados e loading states | 30min |
| 7 | **Projeto Prático:** Dashboard com Next.js | 30min |

📄 Material da Aula 02 *(em breve)*

---

### Aula 03 — Estilização, Integração com API e Deploy

| # | Tópico | Duração |
|---|--------|---------|
| 1 | Tailwind CSS no Next.js | 40min |
| 2 | Hooks essenciais: `useEffect`, `useRef`, Context API | 40min |
| 3 | Formulários e validação | 30min |
| 4 | Integração com API REST externa (ex: Laravel) | 40min |
| 5 | Build, otimização e deploy (Vercel) | 20min |
| 6 | **Projeto Final:** Dashboard completo com API real | 50min |

📄 Material da Aula 03 *(em breve)*

---

## 🏗️ Projeto do Curso: Dashboard de Monitoramento de Servidores

Ao longo das 3 aulas, construímos um **Dashboard de Monitoramento de Servidores Linux**, simulando um ambiente corporativo de infraestrutura.

### Funcionalidades

- ✅ Listagem de servidores com status visual (online/warning/offline)
- ✅ Filtro por status
- ✅ Busca por nome em tempo real
- ✅ Ordenação por consumo de CPU
- ✅ Cards de resumo com contadores
- ✅ Interface dark mode profissional
- 🔲 Rotas dinâmicas por servidor (Aula 02)
- 🔲 API Routes com dados do backend (Aula 02)
- 🔲 Estilização com Tailwind CSS (Aula 03)
- 🔲 Integração com API Laravel (Aula 03)
- 🔲 Deploy na Vercel (Aula 03)

---

## 🚀 Como Rodar o Projeto

```bash
# Clonar o repositório
git clone git@github.com:kaisdevelopment/flexxo-react-next-js.git
cd flexxo-react-next-js

# Instalar dependências
npm install

# Rodar em desenvolvimento
npm run dev

# Acessar
# http://localhost:5173
```

---

## 📂 Estrutura do Repositório

```
flexxo-react-next-js/
├── README.md                          ← Este arquivo
├── aulas/
│   ├── aula-01-js-moderno-react.md    ← Material da Aula 01
│   ├── aula-02-nextjs.md              ← (em breve)
│   └── aula-03-deploy.md              ← (em breve)
├── src/
│   ├── App.jsx
│   ├── main.jsx
│   ├── index.css
│   ├── data/
│   │   └── servidores.js
│   └── components/
│       ├── Header.jsx
│       ├── Footer.jsx
│       ├── ResumoCards.jsx
│       ├── StatusFilter.jsx
│       ├── SearchBar.jsx
│       ├── ServerCard.jsx
│       └── OrderCpu.jsx
└── package.json
```

---

## 🧑‍🏫 Instrutor

**Wiliam** — Instrutor de TI na Flexxo, Polo Caxias do Sul.

---

## 📝 Licença

Material didático de uso exclusivo dos alunos da escola Flexxo.
