---
theme: white
highlightTheme: github
---
# React

ITS 2024-25

---

## Le basi

--

```html
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta 
      name="viewport" 
      content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  </head>
  <body>
    <div class="content">Content</div>
  </body>
</html>
```

--

```css
body {
  padding: 0;
  margin: 0;
  background: #fff;
}

.content {
  max-width: 900px;
  margin: 0 auto;
}
```

--

```js
const el = document.querySelector('.content');

el.innerHTML = "Contenuto aggiornato";
```

- `innerHTML` è altamente sconsigliato per ragioni di **sicurezza**

---

## Obiettivi

- modificare il contenuto della pagina
- reagire agli eventi che si verificano (es: click)

---

## React

- sintassi facile e **sicura** per scrivere HTML usando javascript
- **reactive**: permette di modificare l'HTML in seguito ad eventi

---

## JSX

- sintassi simile ad HTML
- in realtà è javascript "mascherato"

--

```jsx
<MyComponent textVariable="Contenuto da inserire" />
```

- Componente con l'iniziale maiuscola
- attributi `camelCase`
- gli attributi possono contenere anche numeri, booleani, funzioni, array...
  - *(in HTML gli attributi sono sempre stringhe)*

--

```jsx
function MyComponent(props) {
  return (
    <div className="content">
      {props.textVariable}
    </div>
  )
}
```

- gli attributi in react si chiamano **props**


--

```jsx
const Page = () => (
  <Layout>
    <Header>
      <Logo img="./company.png" />
      <Navigation activePage="home" />
      <Search />
    </Header>
    <Container>
      <MyComponent textVariable="Contenuto principale" />
    </Container>
  </Layout>
);
```

- composition

---

## Reactive

--

```jsx  {data-trim data-line-numbers="|2-6"}
<Search 
  onSearch={async (searchQuery) => {
    const searchResults = await 
      fetch('http://api.com/search?' + searchQuery);
    setContent(searchResults);
  }} 
/>
```

- le funzioni sono first class objects
- posso passare una funzione come parametro di un'altra

--

```jsx {data-trim data-line-numbers="|1|3"}
const Search = ({ onSearch }) => {
  return (
    <form onSubmit={ () => onSearch(what) }>
      Cerca:
      <input type="text" name="what" />
    </form>
  )
}
```

> pseudo-code: il form non funziona così

--

```jsx  {data-trim data-line-numbers="|5"}
<Search 
  onSearch={async (searchQuery) => {
    const searchResults = await 
      fetch('http://api.com/search?' + searchQuery);
    setContent(searchResults);
  }} 
/>
```

--

```jsx {data-trim data-line-numbers="|2|8|2|12"}
const Page = () => {
  const [content, setContent] = useState("Contenuto iniziale");
  return (
    <Layout>
        <Search onSearch={async (searchQuery) => {
          const searchResults = await 
            fetch('http://api.com/search?' + searchQuery);
          setContent(searchResults);
        }}/>
      </Header>
      <Container>
        <MyComponent textVariable={content} />
      </Container>
    </Layout>
  );
}

--

## Approfondimenti

- [React.dev](https://react.dev/learn)


---

## Tips

```jsx
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```

- operatori ternari per il conditional rendering

--

```jsx
<div>
  {isLoggedIn && <AdminPanel />}
</div>
```

- se non ti serve `else`, usa l'operatore logico `&&`

--

```jsx
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

- usa le funzioni degli array

--

```jsx
function MyButton() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(count + 1)}>
      Clicked {count} times
    </button>
  );
}

export default function MyApp() {
  return (
    <MyButton />
    <MyButton />
  );
}
```

- ogni componente ha il suo stato

--

```jsx
function MyButton({count, setCount}) {
  return (
    <button onClick={() => setCount(count + 1)}>
      Clicked {count} times
    </button>
  );
}

export default function MyApp() {
  const [count, setCount] = useState(0);
  return (
    <MyButton count={count}, setCount={setCount}/>
    <MyButton count={count}, setCount={setCount}/>
  );
}
```

- stato condiviso tra più componenti

---

## Errori da evitare

--

```jsx {data-trim data-line-numbers="4"}
function MyButton() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => { count = count + 1 }}> // NO!
      Clicked {count} times
    </button>
  );
}
```

- **mai** modificare lo stato direttamente, utilizza sempre il `setState` (qui chiamato `setCount`)

--

```jsx {data-trim data-line-numbers="1,3"}
function MyButton({count}) {
  return (
    <button onClick={() => { count = count + 1 }}> // NO!
      Clicked {count} times
    </button>
  );
}
```

- **rispetta** il flusso di dati, sempre dall'alto verso il basso. Non modificare una proprietà di un componente di livello superiore (in questo esempio: una `prop`) 

---

## Thinking in React

[React.dev](https://react.dev/learn/thinking-in-react)

--

![thinking in react](./assets/thinking-in-react.png)

- disegna una **gerarchia di componenti**

--

- costruisci una versione **statica** in React

--

- progetta una rappresentazione minima ma completa dello stato
- identifica dove implementare lo stato
  - quali componenti devono gestire lo stato?
  - qual è il primo genitore comune di due componenti che devono condividere lo stesso stato?
  - **lift state up**

--

- implementa le prop che contengono le funzioni di callback (**inverse data flow**)

---

## Inizializzare l'applicazione

```html {data-trim data-line-numbers="4"}
<html>
  <head></head>
  <body>
    <div id="app"></div>
  </body>
</html>
```

```jsx {data-trim data-line-numbers="3,4"}
import { createRoot } from 'react-dom/client';
import { Page } from './Page.jsx'
const root = createRoot(document.getElementById('app'));
root.render(<Page />);
```