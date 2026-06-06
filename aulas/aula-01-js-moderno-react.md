# 📘 AULA 01 — JavaScript Moderno (ES6+) e Introdução ao React

**Curso:** Desenvolvimento Front-End Moderno com React e Next.js
**Escola:** Flexxo — Polo Caxias do Sul
**Formato:** Mentoria 1:1 | **Carga horária:** 4h

---

## 🎯 Objetivos da Aula

Ao final desta aula, o aluno será capaz de:

- Dominar os recursos essenciais do JavaScript moderno (ES6+)
- Entender o que é React e por que ele domina o mercado front-end
- Criar componentes funcionais com JSX
- Compreender props, estado e renderização condicional
- Construir um mini-dashboard funcional com React puro

---

## 1. Por que JavaScript Moderno?

O JavaScript evoluiu radicalmente a partir do ES6 (2015). O que era uma linguagem de scripts simples virou a **base de todo o ecossistema web moderno** — front, back (Node.js), mobile (React Native), desktop (Electron).

### Analogia para quem vem do Laravel/PHP

O PHP também evoluiu: do PHP 5 "procedural" para o PHP 8 com typed properties, match, arrow functions, enums. O JavaScript passou pela **mesma revolução**, e entender o JS moderno é pré-requisito absoluto para trabalhar com React.

---

## 2. JavaScript Moderno — ES6+ Essencial

### 2.1 — `var` vs `let` vs `const`

```javascript
// var → EVITE. Escopo de função, permite redeclaração, gera bugs
var nome = "Servidor";
var nome = "Outro"; // sem erro — perigoso!

// let → variável que pode ser reatribuída
let contador = 0;
contador = 1; // OK

// const → não pode ser reatribuída (USE POR PADRÃO)
const API_URL = "https://api.exemplo.com";
// API_URL = "outro"; // ❌ ERRO!
```

> **⚠️ Atenção:** `const` impede reatribuição, **não** impede mutação de objetos/arrays:

```javascript
const servidor = { nome: "srv-01" };
servidor.nome = "srv-02"; // ✅ Funciona! O objeto é mutável.
// servidor = {};          // ❌ ERRO! Não pode reatribuir.
```

**Regra de ouro:** Use `const` por padrão. Só use `let` se precisar reatribuir.

---

### 2.2 — Arrow Functions

```javascript
// Função tradicional
function somar(a, b) {
  return a + b;
}

// Arrow function — forma moderna
const somar = (a, b) => a + b;

// Com corpo de bloco (quando tem mais de uma linha)
const calcularDesconto = (preco, percentual) => {
  const desconto = preco * (percentual / 100);
  return preco - desconto;
};

// Um parâmetro → parênteses opcionais
const dobrar = n => n * 2;
```

**Analogia Laravel:** É como a `fn()` do PHP 7.4+:

```php
// PHP
$somar = fn($a, $b) => $a + $b;

// JavaScript
const somar = (a, b) => a + b;
```

---

### 2.3 — Template Literals

```javascript
const nome = "Juares";
const servidor = "srv-web-01";

// Concatenação antiga (evite)
const msg1 = "Olá, " + nome + "! Servidor: " + servidor;

// Template literal — moderno e legível
const msg2 = `Olá, ${nome}! Servidor: ${servidor}`;

// Suporta expressões
const msg3 = `CPU está em ${87 > 80 ? "ALERTA" : "normal"}`;

// Suporta múltiplas linhas
const html = `
  <div>
    <h1>${nome}</h1>
    <p>Servidor: ${servidor}</p>
  </div>
`;
```

---

### 2.4 — Destructuring

O destructuring é como "abrir uma caixa e pegar exatamente o que precisa".

**Objetos:**

```javascript
const servidor = {
  nome: "srv-web-01",
  ip: "192.168.1.10",
  status: "online",
  cpu: 45,
  ram: 62,
};

// Sem destructuring
const nome = servidor.nome;
const ip = servidor.ip;

// Com destructuring — limpo e direto
const { nome, ip, status } = servidor;

// Renomeando variáveis
const { nome: hostname, ip: endereco } = servidor;

// Valor padrão
const { porta = 22 } = servidor; // porta = 22 (não existe no objeto)
```

**Arrays:**

```javascript
const sistemas = ["Ubuntu", "Rocky Linux", "Debian", "CentOS"];

// Sem destructuring
const primeiro = sistemas[0];

// Com destructuring
const [primeiro, segundo] = sistemas;
// primeiro = "Ubuntu", segundo = "Rocky Linux"

// Pular elementos
const [, , terceiro] = sistemas; // terceiro = "Debian"
```

**Analogia Laravel:** Parecido com o `list()` do PHP:

```php
// PHP
[$nome, $ip] = ["srv-01", "192.168.1.10"];
```

---

### 2.5 — Spread e Rest Operators (`...`)

**Spread → "espalha" os elementos:**

```javascript
// Arrays
const distros = ["Ubuntu", "Debian"];
const todasDistros = [...distros, "Rocky", "Fedora"];
// ["Ubuntu", "Debian", "Rocky", "Fedora"]

// Objetos
const configBase = { porta: 22, protocolo: "SSH" };
const configProd = { ...configBase, porta: 2222, ambiente: "produção" };
// { porta: 2222, protocolo: "SSH", ambiente: "produção" }
```

> Note que a `porta` foi sobrescrita. A **última declaração vence**.

**Rest → "agrupa" o restante:**

```javascript
// Em destructuring
const { nome, ...detalhes } = servidor;
// nome = "srv-web-01"
// detalhes = { ip: "192.168.1.10", status: "online", cpu: 45, ram: 62 }

// Em parâmetros de função
const logInfo = (principal, ...extras) => {
  console.log(`Principal: ${principal}`);
  console.log(`Extras:`, extras);
};
logInfo("erro crítico", "disco cheio", "backup falhou");
```

---

### 2.6 — Operadores Modernos

```javascript
// Nullish Coalescing (??) — usa fallback SÓ se for null/undefined
const porta = config.porta ?? 3000;
const nome = usuario.nome ?? "Anônimo";

// Diferença para || (OR lógico):
const valor1 = 0 || 10;   // 10 (0 é falsy!)
const valor2 = 0 ?? 10;   // 0 (0 NÃO é null/undefined)

// Optional Chaining (?.) — acesso seguro a propriedades
const cidade = usuario?.endereco?.cidade;
// Se 'endereco' não existir → undefined (sem erro!)

// Combinando os dois
const cep = usuario?.endereco?.cep ?? "CEP não informado";
```

**Analogia Laravel:** `??` é idêntico ao `??` do PHP. `?.` funciona como o helper `optional()`.

---

### 2.7 — Métodos de Array (Os mais usados no React)

Esses métodos são **fundamentais** para trabalhar com React, porque é assim que renderizamos listas.

```javascript
const servidores = [
  { nome: "srv-web-01", status: "online", cpu: 45 },
  { nome: "srv-db-01", status: "online", cpu: 72 },
  { nome: "srv-backup", status: "offline", cpu: 0 },
  { nome: "srv-app-01", status: "warning", cpu: 89 },
];

// .map() → transforma cada item (MUITO usado no React)
const nomes = servidores.map(s => s.nome);
// ["srv-web-01", "srv-db-01", "srv-backup", "srv-app-01"]

// .filter() → filtra por condição
const online = servidores.filter(s => s.status === "online");

// .find() → encontra o primeiro que bate
const critico = servidores.find(s => s.cpu > 80);

// .some() → algum atende?
const temOffline = servidores.some(s => s.status === "offline"); // true

// .every() → todos atendem?
const todosOnline = servidores.every(s => s.status === "online"); // false

// .reduce() → acumula valor
const cpuTotal = servidores.reduce((soma, s) => soma + s.cpu, 0); // 206
const cpuMedia = cpuTotal / servidores.length; // 51.5

// Encadeamento — filtrar + transformar
const nomesOnline = servidores
  .filter(s => s.status === "online")
  .map(s => s.nome);
// ["srv-web-01", "srv-db-01"]
```

---

### 2.8 — Async/Await e Promises

```javascript
// Fetch com .then() — funciona, mas fica verboso
fetch("https://api.exemplo.com/servidores")
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error("Erro:", error));

// Async/Await — moderno, limpo e legível
const buscarServidores = async () => {
  try {
    const response = await fetch("https://api.exemplo.com/servidores");
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Erro ao buscar servidores:", error);
    return [];
  }
};
```

**Analogia Laravel:** `await` é como se fosse síncrono mas sem travar a thread. Pense como o `Http::get()` do Laravel, mas sem bloquear.

---

### 2.9 — Modules (import/export)

```javascript
// utils/helpers.js — exportando
export const formatarIP = (ip) => `[${ip}]`;
export const STATUS = { ONLINE: "online", OFFLINE: "offline" };

// Exportação default (uma por arquivo)
export default function calcularUptime(horas) {
  return `${Math.floor(horas / 24)} dias`;
}

// App.js — importando
import calcularUptime, { formatarIP, STATUS } from "./utils/helpers";
```

**Analogia Laravel:** É como o `use App\Models\Servidor;` do PHP — importa o que precisa, de onde precisa.

---

## 3. Introdução ao React

### 3.1 — O que é React?

React é uma **biblioteca JavaScript para construir interfaces de usuário** criada pelo Facebook (Meta) em 2013. Hoje é a biblioteca front-end **mais usada no mundo**.

| Característica | Descrição |
|---|---|
| **Baseado em Componentes** | A UI é dividida em peças reutilizáveis |
| **Declarativo** | Você descreve O QUE quer, não COMO fazer |
| **Virtual DOM** | Atualiza apenas o que mudou (performance) |
| **Unidirecional** | Dados fluem de pai → filho (previsível) |

### Analogia com Laravel

| Laravel | React |
|---|---|
| Blade Component | React Component |
| `@include('partial')` | `<MeuComponente />` |
| `@props(['titulo'])` | `function Comp({ titulo })` |
| Blade renderiza no servidor | React renderiza no cliente |
| `@if`, `@foreach` | `{condicao && ...}`, `.map()` |

---

### 3.2 — JSX: O Template Engine do React

JSX mistura JavaScript com HTML. Parece estranho no início, mas é **extremamente poderoso**.

**Regras do JSX:**

| HTML | JSX |
|---|---|
| `class="btn"` | `className="btn"` |
| `for="email"` | `htmlFor="email"` |
| `style="color: red"` | `style={{ color: 'red' }}` |
| `onclick="fn()"` | `onClick={fn}` |
| Pode ter múltiplas raízes | **Precisa de UM wrapper** |
| Tags podem ser abertas | **Toda tag deve fechar:** `<img />` |

```jsx
// ✅ Correto — um elemento raiz
const Card = () => (
  <div>
    <h2>Título</h2>
    <p>Texto</p>
  </div>
);

// ✅ Correto — Fragment (wrapper invisível)
const Card = () => (
  <>
    <h2>Título</h2>
    <p>Texto</p>
  </>
);

// ❌ ERRO — múltiplas raízes
const Card = () => (
  <h2>Título</h2>
  <p>Texto</p>
);
```

---

### 3.3 — Componentes Funcionais

No React moderno, **todo componente é uma função** que retorna JSX.

```jsx
// Componente simples
function Saudacao() {
  return <h1>Olá, mundo!</h1>;
}

// Como arrow function (mais comum em projetos reais)
const Saudacao = () => <h1>Olá, mundo!</h1>;

// Usando o componente
const App = () => (
  <div>
    <Saudacao />
    <Saudacao />
  </div>
);
```

> **Regra:** Nome de componente sempre em **PascalCase** (`ServerCard`, não `serverCard`).

---

### 3.4 — Props: Passando Dados para Componentes

Props são como os **parâmetros** de um componente. Funcionam exatamente como os `@props` do Blade.

```jsx
// Definindo o componente com props
const ServerCard = ({ nome, ip, status }) => {
  return (
    <div className="card">
      <h3>{nome}</h3>
      <p>IP: {ip}</p>
      <p>Status: {status}</p>
    </div>
  );
};

// Usando e passando dados
const App = () => (
  <div>
    <ServerCard nome="srv-web-01" ip="192.168.1.10" status="online" />
    <ServerCard nome="srv-db-01" ip="192.168.1.20" status="offline" />
  </div>
);
```

**Props com valor padrão:**

```jsx
const ServerCard = ({ nome, status = "desconhecido" }) => (
  <div>
    <h3>{nome}</h3>
    <p>Status: {status}</p>
  </div>
);

// Se não passar status, usa "desconhecido"
<ServerCard nome="srv-teste" />
```

---

### 3.5 — Estado com `useState`

O estado é o que torna um componente **interativo**. Quando o estado muda, o React re-renderiza automaticamente.

```jsx
import { useState } from "react";

const Contador = () => {
  const [contagem, setContagem] = useState(0);
  //     ↑ valor       ↑ função       ↑ valor inicial

  return (
    <div>
      <p>Contagem: {contagem}</p>
      <button onClick={() => setContagem(contagem + 1)}>
        +1
      </button>
      <button onClick={() => setContagem(0)}>
        Resetar
      </button>
    </div>
  );
};
```

**Exemplo mais real — Filtro de status:**

```jsx
import { useState } from "react";

const FiltroServidor = () => {
  const [filtro, setFiltro] = useState("todos");

  const servidores = [
    { nome: "srv-web-01", status: "online" },
    { nome: "srv-db-01", status: "online" },
    { nome: "srv-backup", status: "offline" },
  ];

  const filtrados = filtro === "todos"
    ? servidores
    : servidores.filter(s => s.status === filtro);

  return (
    <div>
      <div>
        <button onClick={() => setFiltro("todos")}>Todos</button>
        <button onClick={() => setFiltro("online")}>Online</button>
        <button onClick={() => setFiltro("offline")}>Offline</button>
      </div>
      <p>Filtro ativo: {filtro} ({filtrados.length} resultados)</p>
      <ul>
        {filtrados.map(s => (
          <li key={s.nome}>{s.nome} — {s.status}</li>
        ))}
      </ul>
    </div>
  );
};
```

---

### 3.6 — Renderização Condicional

```jsx
const StatusIndicator = ({ status }) => {
  return (
    <span className={status === "online" ? "text-green" : "text-red"}>
      {status === "online" ? "🟢 Online" : "🔴 Offline"}
    </span>
  );
};

// Com && (short-circuit) — renderiza só se a condição for true
const Alerta = ({ cpu }) => (
  <div>
    <p>CPU: {cpu}%</p>
    {cpu > 80 && <p className="alerta">⚠️ CPU em estado crítico!</p>}
  </div>
);

// Múltiplas condições
const StatusBadge = ({ status }) => {
  const config = {
    online:  { emoji: "🟢", texto: "Operacional" },
    warning: { emoji: "🟡", texto: "Atenção" },
    offline: { emoji: "🔴", texto: "Fora do ar" },
  };

  const { emoji, texto } = config[status] ?? config.offline;

  return <span>{emoji} {texto}</span>;
};
```

---

### 3.7 — Renderização de Listas com `.map()`

```jsx
const ListaServidores = () => {
  const servidores = [
    { id: 1, nome: "srv-web-01", status: "online", cpu: 45 },
    { id: 2, nome: "srv-db-01", status: "online", cpu: 72 },
    { id: 3, nome: "srv-backup", status: "offline", cpu: 0 },
    { id: 4, nome: "srv-app-01", status: "warning", cpu: 89 },
  ];

  return (
    <div>
      <h2>Servidores ({servidores.length})</h2>
      {servidores.map(servidor => (
        <div key={servidor.id} className="card">
          <h3>{servidor.nome}</h3>
          <p>Status: {servidor.status}</p>
          <p>CPU: {servidor.cpu}%</p>
          {servidor.cpu > 80 && <p>⚠️ Atenção!</p>}
        </div>
      ))}
    </div>
  );
};
```

> **⚠️ Sempre use `key`!** O React precisa de uma key única em cada item de lista para saber o que re-renderizar. Use o `id` do dado, **nunca** o índice do array.

---

## 4. Projeto Prático: Dashboard de Servidores

### 4.1 — Criando o Projeto React

```bash
# Criar com Vite (ferramenta moderna, rápida)
npm create vite@latest dashboard-servidores -- --template react
cd dashboard-servidores
npm install
npm run dev
```

Acesse **http://localhost:5173** — React rodando!

### 4.2 — Estrutura do Projeto

```
dashboard-servidores/
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
```

---

### 4.3 — Dados Mock

`src/data/servidores.js`:

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

### 4.4 — Componente Header

`src/components/Header.jsx`:

```jsx
const Header = () => (
  <header style={{
    backgroundColor: "#111827",
    borderBottom: "1px solid #1F2937",
    padding: "16px 24px",
    display: "flex",
    justifyContent: "space-between",
    alignItems: "center",
  }}>
    <h1 style={{ fontSize: "20px", fontWeight: "bold", color: "#10B981" }}>
      🖥️ Server Monitor
    </h1>
    <span style={{ fontSize: "14px", color: "#6B7280" }}>
      Flexxo Infra
    </span>
  </header>
);

export default Header;
```

---

### 4.5 — Componente ResumoCards

`src/components/ResumoCards.jsx`:

```jsx
const CardResumo = ({ valor, label, cor }) => (
  <div style={{
    backgroundColor: `${cor}15`,
    border: `1px solid ${cor}40`,
    borderRadius: "12px",
    padding: "16px",
    textAlign: "center",
  }}>
    <p style={{ fontSize: "32px", fontWeight: "bold", color: cor }}>
      {valor}
    </p>
    <p style={{ fontSize: "14px", color: "#9CA3AF" }}>{label}</p>
  </div>
);

const ResumoCards = ({ servidores }) => {
  const online = servidores.filter(s => s.status === "online").length;
  const warning = servidores.filter(s => s.status === "warning").length;
  const offline = servidores.filter(s => s.status === "offline").length;

  return (
    <div style={{
      display: "grid",
      gridTemplateColumns: "1fr 1fr 1fr",
      gap: "16px",
      marginBottom: "24px",
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

### 4.6 — Componente SearchBar

`src/components/SearchBar.jsx`:

```jsx
const SearchBar = ({ value, onChange }) => (
  <input
    type="text"
    value={value}
    onChange={(e) => onChange(e.target.value)}
    placeholder="🔍 Buscar servidor por nome..."
    style={{
      width: "100%",
      padding: "12px 16px",
      borderRadius: "8px",
      border: "1px solid #374151",
      backgroundColor: "#111827",
      color: "#F3F4F6",
      fontSize: "14px",
      outline: "none",
    }}
  />
);

export default SearchBar;
```

---

### 4.7 — Componente StatusFilter

`src/components/StatusFilter.jsx`:

```jsx
import { useState } from "react";

const StatusFilter = ({ onFilter }) => {
  const [ativo, setAtivo] = useState("todos");
  const opcoes = ["todos", "online", "warning", "offline"];

  const handleClick = (opcao) => {
    setAtivo(opcao);
    onFilter(opcao);
  };

  return (
    <div style={{ display: "flex", gap: "8px" }}>
      {opcoes.map(opcao => (
        <button
          key={opcao}
          onClick={() => handleClick(opcao)}
          style={{
            padding: "8px 16px",
            borderRadius: "8px",
            border: "none",
            cursor: "pointer",
            fontWeight: "500",
            fontSize: "14px",
            backgroundColor: ativo === opcao ? "#10B981" : "#1F2937",
            color: ativo === opcao ? "#FFF" : "#9CA3AF",
            transition: "all 0.2s",
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

### 4.8 — Componente OrderCpu

`src/components/OrderCpu.jsx`:

```jsx
const OrderCpu = ({ value, onChange }) => {
  return (
    <select
      value={value}
      onChange={(e) => onChange(e.target.value)}
      style={{
        padding: "8px",
        borderRadius: "8px",
        border: "1px solid #1F2937",
        backgroundColor: "#111827",
        color: "#F3F4F6",
        fontFamily: "inherit",
        cursor: "pointer",
      }}
    >
      <option value="default">Ordenar por CPU</option>
      <option value="asc">CPU ↑ Crescente</option>
      <option value="desc">CPU ↓ Decrescente</option>
    </select>
  );
};

export default OrderCpu;
```

---

### 4.9 — Componente ServerCard

`src/components/ServerCard.jsx`:

```jsx
const statusConfig = {
  online:  { cor: "#10B981", emoji: "🟢" },
  warning: { cor: "#F59E0B", emoji: "🟡" },
  offline: { cor: "#EF4444", emoji: "🔴" },
};

const ServerCard = ({ servidor }) => {
  const { nome, ip, os, status, cpu, ram, uptime } = servidor;
  const { cor, emoji } = statusConfig[status] ?? statusConfig.offline;

  return (
    <div style={{
      backgroundColor: "#111827",
      border: `2px solid ${cor}40`,
      borderRadius: "12px",
      padding: "20px",
      transition: "border-color 0.2s",
    }}>
      <div style={{
        display: "flex",
        justifyContent: "space-between",
        alignItems: "start",
        marginBottom: "12px",
      }}>
        <h3 style={{ fontSize: "18px", fontWeight: "bold", color: "#F3F4F6" }}>
          {nome}
        </h3>
        <span style={{
          backgroundColor: `${cor}20`,
          color: cor,
          padding: "4px 10px",
          borderRadius: "6px",
          fontSize: "12px",
          fontWeight: "bold",
          textTransform: "uppercase",
        }}>
          {emoji} {status}
        </span>
      </div>

      <div style={{ fontSize: "14px", color: "#9CA3AF", lineHeight: "1.8" }}>
        <p>📡 IP: <span style={{ color: "#D1D5DB" }}>{ip}</span></p>
        <p>💻 OS: <span style={{ color: "#D1D5DB" }}>{os}</span></p>
        <p>⚡ CPU: <span style={{ color: cpu > 80 ? "#EF4444" : "#D1D5DB" }}>{cpu}%</span></p>
        <p>🧠 RAM: <span style={{ color: ram > 85 ? "#EF4444" : "#D1D5DB" }}>{ram}%</span></p>
        <p>⏱️ Uptime: <span style={{ color: "#D1D5DB" }}>{uptime ?? "N/A"}</span></p>
      </div>

      {(cpu > 80 || ram > 85) && (
        <p style={{
          marginTop: "12px",
          padding: "8px",
          backgroundColor: "#7F1D1D20",
          borderRadius: "6px",
          color: "#FCA5A5",
          fontSize: "13px",
        }}>
          ⚠️ Recurso(s) em estado crítico!
        </p>
      )}
    </div>
  );
};

export default ServerCard;
```

---

### 4.10 — Componente Footer

`src/components/Footer.jsx`:

```jsx
const Footer = () => {
  const ano = new Date().getFullYear();

  return (
    <footer style={{
      borderTop: "1px solid #1F2937",
      padding: "20px 24px",
      marginTop: "48px",
      textAlign: "center",
      color: "#6B7280",
      fontSize: "13px",
    }}>
      <p>© {ano} Flexxo — Polo Caxias do Sul</p>
      <p style={{ marginTop: "4px", fontSize: "12px", color: "#4B5563" }}>
        Dashboard de Monitoramento v1.0
      </p>
    </footer>
  );
};

export default Footer;
```

---

### 4.11 — App.jsx (Versão Final)

`src/App.jsx`:

```jsx
import { useState } from "react";
import { servidores } from "./data/servidores";
import Header from "./components/Header";
import ResumoCards from "./components/ResumoCards";
import StatusFilter from "./components/StatusFilter";
import SearchBar from "./components/SearchBar";
import ServerCard from "./components/ServerCard";
import Footer from "./components/Footer";
import OrderCpu from "./components/OrderCpu";

const App = () => {
  const [filtro, setFiltro] = useState("todos");
  const [busca, setBusca] = useState("");
  const [ordemCpu, setOrdemCpu] = useState("default");

  // Pipeline completo: status → busca → ordenação
  const servidoresFiltrados = servidores
    .filter(s => filtro === "todos" || s.status === filtro)
    .filter(s => s.nome.toLowerCase().includes(busca.toLowerCase()))
    .sort((a, b) => {
      if (ordemCpu === "asc") return a.cpu - b.cpu;
      if (ordemCpu === "desc") return b.cpu - a.cpu;
      return 0;
    });

  return (
    <div style={{
      backgroundColor: "#030712",
      color: "#F3F4F6",
      minHeight: "100vh",
      fontFamily: "'Segoe UI', sans-serif",
      display: "flex",
      flexDirection: "column",
    }}>
      <Header />
      <main style={{ maxWidth: "960px", margin: "0 auto", padding: "24px", width: "100%", flex: 1 }}>
        <h2 style={{ fontSize: "24px", fontWeight: "bold", marginBottom: "20px" }}>
          📊 Dashboard
        </h2>

        <ResumoCards servidores={servidores} />

        <div style={{ display: "flex", flexDirection: "column", gap: "16px", marginBottom: "24px" }}>
          <SearchBar value={busca} onChange={setBusca} />
          <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", flexWrap: "wrap", gap: "12px" }}>
            <StatusFilter onFilter={setFiltro} />
            <OrderCpu value={ordemCpu} onChange={setOrdemCpu} />
            {busca && (
              <span style={{ fontSize: "14px", color: "#9CA3AF" }}>
                Encontrados: <strong>{servidoresFiltrados.length}</strong>
              </span>
            )}
          </div>
        </div>

        <div style={{
          display: "grid",
          gridTemplateColumns: "1fr 1fr",
          gap: "16px",
        }}>
          {servidoresFiltrados.map(servidor => (
            <ServerCard key={servidor.id} servidor={servidor} />
          ))}
        </div>

        {servidoresFiltrados.length === 0 && (
          <p style={{ textAlign: "center", color: "#6B7280", marginTop: "40px" }}>
            {busca
              ? `Nenhum servidor encontrado para "${busca}"`
              : `Nenhum servidor com status "${filtro}"`}
          </p>
        )}
      </main>
      <Footer />
    </div>
  );
};

export default App;
```

---

### 4.12 — CSS Global

`src/index.css`:

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #030712;
}
```

---

## 5. Conceitos Aplicados — Resumo Visual

| Conceito JS/React | Onde aparece no projeto |
|---|---|
| `const` / `let` | Em todo lugar |
| Arrow functions | Todos os componentes |
| Template literals | Strings dinâmicas (`${cor}40`) |
| Destructuring | Props dos componentes, dados |
| Spread (`...`) | Pode expandir no futuro |
| `??` Nullish Coalescing | `uptime ?? "N/A"` no ServerCard |
| `?.` Optional Chaining | `statusConfig[status]` com fallback |
| `.map()` | Lista de servidores, lista de botões |
| `.filter()` | Filtro de status e busca por nome |
| `.sort()` | Ordenação por CPU |
| `useState` | Filtro, busca e ordenação no App |
| Renderização condicional | Alerta de CPU, mensagem vazia |
| Props | Todos os componentes recebem dados |
| Componentes | Header, Footer, ResumoCards, StatusFilter, SearchBar, ServerCard, OrderCpu |

---

## 6. Checklist de Conceitos da Aula 01

| # | Conceito | Status |
|---|---|---|
| 1 | `const`, `let` — quando usar cada um | ✅ |
| 2 | Arrow Functions | ✅ |
| 3 | Template Literals | ✅ |
| 4 | Destructuring (objetos e arrays) | ✅ |
| 5 | Spread / Rest (`...`) | ✅ |
| 6 | `??` e `?.` (Nullish Coalescing / Optional Chaining) | ✅ |
| 7 | `.map()`, `.filter()`, `.find()`, `.reduce()` | ✅ |
| 8 | Async/Await (conceito) | ✅ |
| 9 | Import/Export de módulos | ✅ |
| 10 | O que é React e JSX | ✅ |
| 11 | Componentes funcionais | ✅ |
| 12 | Props e valores padrão | ✅ |
| 13 | Estado com `useState` | ✅ |
| 14 | Renderização condicional | ✅ |
| 15 | Renderização de listas com `.map()` e `key` | ✅ |
| 16 | Projeto Dashboard funcionando | ✅ |

---

## 🔭 Próxima Aula

**Aula 02 — Next.js: App Router, Server Components e Rotas Dinâmicas**

Vamos levar todo esse conhecimento de React para dentro do Next.js, adicionando:

- Roteamento automático por arquivos
- Server Components vs Client Components
- Rotas dinâmicas (`/servidor/[id]`)
- API Routes integradas
- Fetch de dados com loading states

---

## 📚 Referências

- [MDN — JavaScript](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript)
- [React — Documentação Oficial](https://react.dev)
- [Vite — Build Tool](https://vitejs.dev)
- [Next.js — Documentação](https://nextjs.org/docs)

---

*Material didático — Escola Flexxo, Polo Caxias do Sul © 2026*
