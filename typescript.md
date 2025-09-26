---
theme: black
highlightTheme: github-dark
---

# Typescript

---

## Javascript è Typescript

(valido)

--

- Typescript è un *superset* di javascript
- Codice javascript corretto = codice typescript corretto
- Typescript aggiunge *static type checking* a javascript

--

- estensione dei file javascript: *.js* / *.jsx*
- estensione dei file typescript: *.ts* / *.tsx*

--

`npm install typescript`

--

## Typescript non viene eseguito direttamente

`tsc hello.ts`

Genera un file `hello.js` che può essere eseguito in un browser o in Node.js (o altro ambiente javascript).

--

## tsc: typescript compiler

Durante il "transpiling" di un file, typescript controlla che il codice sia coerente e riporta errori e warning se trova qualche errore.

---

## Tipi semplici

```ts
const nome: string = "Mario";
const today: Date = new Date();
const eta: number = 23;
const presente: boolean = false;
const classe: "mouse" | "samba" = "mouse"; // string literal union type
const preferenze: string[] = ["react", "htnl"] // array di stringhe
```

--

## Null e undefined

- `null`: è un valore, di tipo nullo
- `undefined`: non è un valore, viene restituito quando una variabile non è presente

--

## Null

```ts
const nickName: string | null = null;
```

--

## Undefined

```ts
type Person = {
  name: string,
  nickName: string | undefined // same as: nickName?: string
}
```

--

## Any e Unknown

```ts
function f1(a: any) {
  a.b(); // OK
}

function f2(a: unknown) {
  a.b();
// error: 'a' is of type 'unknown'.
}
```

Entrambi i tipi accettano qualsiasi cosa, ma con *unknown* non si può fare nulla

--

## Uso di Unknown

```ts
interface Box {
  contents: unknown;
}
 
let x: Box = {
  contents: "hello world", // accetta qualsiasi tipo
};
```

---

## Tipi meno semplici

--

## Tuple

Un array di cui è conosciuto il numero di elementi e il tipo di ciascuno di essi

```ts
type SetStateFn = (v: string) => string; 
// semplificazione, in realtà il tipo di React è più complesso

const [state, setState]: [string, SetStateFn] = useState("initial value");
```

--

## Union types

```ts
const dataDiNascita: string | Date = "10/03/2001"; 
// potrebbe anche essere: new Date(2001, 3, 10);

const id: string | number = "15"; // stringa
// sarebbe valido usare anche il numero 15 

const preferenze: string | string[] = "giallo"; 
// anche ["giallo", "blu"] è valido
```

--

## Union types di stringhe

```ts

const alignment: "left" | "center" | "right" = "center";

```

Utile quando il valore stringa deve essere ridotto a un set finito di opzioni

--

## Generics

```ts
interface Box<Type> {
  contents: Type;
}

let box1: Box<string> = { contents: "hello world" };
let box2: Box<number> = { contents: 20 };
```

Invece di usare *unknown*, usiamo un tipo generico che accetta un parametro per descrivere una proprietà interna

---

## Oggetti

```ts {data-trim data-line-numbers="|2-3|5-6"}
const coordinates: {
  latitude: number,
  longitude: number
} = {
  latitude: 0.543,
  longitude: 10.345
}
```

--

## Type aliases

Un modo più leggibile per rappresentare l'oggetto precedente:

```ts
type Coord = {latitude: number, longitude: number};

const coordinates: Coord = {
  latitude: 0.543,
  longitude: 10.345
}
```

--

## Proprietà opzionali

```ts
type Person = {
  firstName: string,
  lastName: string,
  nickName?: string // questo parametro è opzionale (può essere undefined)
}
```

--

## Intersection types

```ts
type Person = {
  firstName: string,
  lastName: string,
}

type Student = Person & {
  school: string
}

const Mario: Student = {
  firstName: "Mario",
  lastName: "Rossi",
  school: "ITS"
}
```

---

## Funzioni: tipizzazione dei parametri

```ts
// Parameter type annotation
function greet(name: string) {
  console.log("Hello, " + name.toUpperCase() + "!!");
}
```

--

## Funzioni: return type

```ts
function getFavoriteNumber(): number {
  return 26;
}
```

--

## Funzioni asincrone

```ts
async function getFavoriteNumber(): Promise<number> {
  return 26;
}
```

--

## Arrow functions

```ts
type MultiplyFn = (a: number, b: number) => number;

const multiply: MultiplyFn = (a, b) => {
  return a * b;
}

const result = multiply(2, 3); // result is a number
```

--

## Generics

```ts
function firstElement<Type>(arr: Type[]): Type | undefined {
  return arr[0];
}

// s is of type 'string'
const s = firstElement(["a", "b", "c"]);

// n is of type 'number'
const n = firstElement([1, 2, 3]);

// u is of type undefined
const u = firstElement([]);
```

Il tipo di valore ritornato non è noto a priori, dipende dall'input

--

## Rest parameters & arguments

```ts
function multiply(n: number, ...m: number[]) {
  return m.map((x) => n * x);
}

const a = multiply(10, 2, 3, 4, 5); // n = 10; m = [2, 3, 4, 5]
// 'a' = [20, 30, 40, 50]
```

---

## Tipi di React


```bash
npm install —D @types/react @types/react-dom
```

React non contiene le definizioni dei tipi, devono essere installate a parte

--

## Component

```tsx {data-trim data-line-numbers="5,11"}
type AppProps = {
  greet: string
}

const App: React.FunctionComponent<AppProps> = ({ greet }) => {
  return <div>Hello {greet}</div>
}

// Il tipo FunctionComponent ha un alias FC, 
// che viene usato più di frequente:
const App = React.FC<AppProps> = (...) => {...} 
```

--

## State

```tsx
const [user, setUser] = useState<User | null>(null);
```

--

## Context

```tsx
import { createContext } from "react";

type ThemeContextType = "light" | "dark";

const ThemeContext = createContext<ThemeContextType>("light");
```

--

## DOM elements

```tsx
const divRef = useRef<HTMLDivElement>(null);
```

--

## DOM events

```tsx
function handleChange(event: React.ChangeEvent<HTMLInputElement>) {
  setValue(event.currentTarget.value);
}
```

--

## Children

```tsx {data-trim data-line-numbers="3"}
type ModalRendererProps = {
  title: string;
  children: React.ReactNode; // accetta copmonenti, stringhe, numeri...
}
```

```tsx {data-trim data-line-numbers="3"}
type ModalRendererProps = {
  title: string;
  children: React.ReactElement; // accetta solo componenti React, più restrittivo
}
```

--

## Style props

```tsx {data-trim data-line-numbers="2"}
type MyComponentProps = {
  style: React.CSSProperties;
}
```

--

## Approfondisci

[https://react.dev/learn/typescript](https://react.dev/learn/typescript)