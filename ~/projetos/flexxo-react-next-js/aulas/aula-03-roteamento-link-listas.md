# 📘 AULA 03 — Navegação no Next.js: Roteamento por Pastas, Link e Listas

> Material didático da Escola Flexxo — Polo Caxias do Sul.
> Foco: como o Next.js cria páginas a partir de pastas, como navegar entre elas com o componente `<Link>` e como renderizar listas com `.map()`.

---

## 🎯 Objetivos da Aula

- Entender o roteamento por pastas (File-Based Routing) do App Router
- Dominar o componente `<Link>` e o conceito de SPA
- Renderizar listas dinâmicas com `.map()` e o uso da `key`
- Saber onde colocar componentes reutilizáveis no projeto

---

## 📋 1. Renderizando Listas com `.map()`

O `.map()` transforma **cada item de um array em um elemento JSX**.

```jsx
const frutas = ['🍎 Maçã', '🍌 Banana', '🍇 Uva'];

{frutas.map((fruta) => <li>{fruta}</li>)}
```

### 🔑 A `key` é obrigatória

Cada item precisa de uma `key` única para o React identificá-lo.

```jsx
{frutas.map((fruta, index) => (
  <li key={index}>{fruta}</li>
))}
```

> ⚠️ Prefira sempre um `id` real à `key={index}`, principalmente quando a lista pode mudar de ordem ou ter itens removidos.

### 🎬 Exemplo completo — Lista de Tarefas

```jsx
'use client';

import { useState } from 'react';

export default function ListaTarefas() {
  const [tarefas, setTarefas] = useState([
    { id: 1, texto: 'Estudar React 📚' },
    { id: 2, texto: 'Tomar café ☕' },
    { id: 3, texto: 'Dar aula 👨‍🏫' },
  ]);

  function adicionar() {
    const nova = {
      id: Date.now(),
      texto: `Tarefa ${tarefas.length + 1} ✨`,
    };
    setTarefas([...tarefas, nova]);
  }

  return (
    <div style={{ padding: '20px' }}>
      <h2>📋 Minhas Tarefas ({tarefas.length})</h2>
      <ul>
        {tarefas.map((tarefa) => (
          <li key={tarefa.id}>{tarefa.texto}</li>
        ))}
      </ul>
      <button onClick={adicionar}>+ Adicionar tarefa</button>
    </div>
  );
}
```

> 💡 Dica: o par perfeito do `.map()` é o `.filter()` para remover itens:
> `setTarefas(tarefas.filter((t) => t.id !== id));`

---

## 📁 2. Roteamento por Pastas (App Router)

**Regra de ouro:** cada `page.jsx` dentro de uma pasta = uma rota (URL).

```
app/
├── page.jsx              →  /              (página inicial)
├── sobre/
│   └── page.jsx          →  /sobre
└── contato/
    └── page.jsx          →  /contato
```

> 📌 O nome da **pasta** vira a URL. O arquivo **sempre** se chama `page.jsx` (minúsculo).

### Exemplo de página

```jsx
// app/sobre/page.jsx
export default function SobrePage() {
  return (
    <main style={{ padding: '30px' }}>
      <h1>📖 Sobre nós</h1>
      <p>Esta página apareceu só por criar a pasta "sobre"! 🚀</p>
    </main>
  );
}
```

| Quero a URL... | Crio o arquivo... |
|----------------|-------------------|
| `/` | `app/page.jsx` |
| `/sobre` | `app/sobre/page.jsx` |
| `/contato` | `app/contato/page.jsx` |
| `/blog/posts` | `app/blog/posts/page.jsx` |

---

## 🔗 3. O Componente `<Link>`

### O problema do `<a href>` tradicional

A tag `<a>` recarrega o site INTEIRO a cada clique: descarta a página, pede tudo de novo ao servidor e remonta do zero (a tela pisca em branco). É lento.

### A solução: SPA + `<Link>`

Apps Next são **SPAs (Single Page Applications)**: o site carrega uma vez e depois troca apenas o **pedaço** que muda. O `<Link>` dá esse comportamento à navegação.

```jsx
import Link from 'next/link';

export default function Menu() {
  return (
    <nav style={{ display: 'flex', gap: '15px' }}>
      <Link href="/">Início</Link>
      <Link href="/sobre">Sobre</Link>
      <Link href="/contato">Contato</Link>
    </nav>
  );
}
```

### `<a>` vs `<Link>`

| Critério | `<a href>` 🐢 | `<Link href>` 🚀 |
|----------|--------------|------------------|
| Recarrega a página? | Sim (pisca) | Não (instantâneo) |
| Velocidade | Lenta | Rápida |
| Mantém o estado do app? | ❌ Perde tudo | ✅ Mantém |
| Pré-carrega a página (prefetch)? | ❌ Não | ✅ Sim |

### 🔮 Prefetch

Quando um `<Link>` aparece na tela, o Next já baixa a página de fundo. No clique, ela abre na hora. Por isso sites em Next são tão rápidos.

### Recursos extras

```jsx
// Link dinâmico com variável
const id = 42;
<Link href={`/produto/${id}`}>Ver produto</Link>

// Desligar prefetch (casos específicos)
<Link href="/sobre" prefetch={false}>Sobre</Link>
```

> 🎯 Regra: link **interno** (seu próprio site) → `<Link>`. Link **externo** (outro site) → `<a>`.

---

## 🗂️ 4. Onde colocar os componentes

### Cenário A — Direto na página

Para um menu de uso único, coloque o `<Link>` direto no `page.jsx`.

### Cenário B — Componente reutilizável (profissional)

Crie um arquivo próprio e importe onde precisar:

```jsx
// app/components/Menu.jsx
import Link from 'next/link';

export default function Menu() {
  return (
    <nav style={{ display: 'flex', gap: '15px' }}>
      <Link href="/">Início</Link>
      <Link href="/sobre">Sobre</Link>
      <Link href="/contato">Contato</Link>
    </nav>
  );
}
```

```jsx
// app/page.jsx
import Menu from './components/Menu';

export default function HomePage() {
  return (
    <main style={{ padding: '30px' }}>
      <Menu />
      <h1>🏠 Página Inicial</h1>
    </main>
  );
}
```

### Estrutura de pastas

```
app/
├── page.jsx                 ← usa o <Menu />
├── components/
│   └── Menu.jsx             ← componente reutilizável
├── sobre/
│   └── page.jsx
└── contato/
    └── page.jsx
```

> ⚠️ Atenção ao caminho do import:
> - De `app/page.jsx` → `'./components/Menu'`
> - De `app/sobre/page.jsx` → `'../components/Menu'` (o `..` sobe uma pasta)

| Quero o menu... | Onde coloco o código |
|-----------------|---------------------|
| Só em 1 página | direto no `page.jsx` dela |
| Reutilizável em várias | `components/Menu.jsx` + importar |
| Em todas automaticamente | `layout.jsx` (próxima aula) |

---

## 📚 Referências

- [Next.js — Link Component](https://nextjs.org/docs/app/api-reference/components/link)
- [Next.js — Routing Fundamentals](https://nextjs.org/docs/app/building-your-application/routing)
- [React — Rendering Lists](https://react.dev/learn/rendering-lists)

---

*Material didático — Escola Flexxo, Polo Caxias do Sul © 2026*
