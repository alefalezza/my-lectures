---
theme: black
highlightTheme: github-dark
---

# Javascript - le basi

---

## Const

Il valore di una costante non può essere modificato

```js
const invariabile = 5;
invariabile = 6; // NO! 
// TypeError: Assignment to constant variable.
```

--

## Const

Se la costante è un oggetto o un array,
le sue proprietà o i suoi elementi
possono essere modificati!

```js
const nomeCostante = {}; // Oggetto

nomeCostante.proprieta = 1; // OK: 
// ho modificato una proprietà dell'oggetto, 
// ma non l'oggetto in sè
```

--

## Let

Può cambiare nel tempo

```js
let nomeLet = "valore";

nomeLet = {
  proprieta: "valore",
  altra: 123
};
```

Si può fare, ma non è consigliato modificare il tipo di dato di una variabile!

--

## Var

Sintassi valida ma non più in uso

```js
var nomeVariabile = "valore";
```

---

## Oggetti

```js
const myObject = {
  proprieta1: "valore",
  proprieta2: "altro valore"
};
```

Accedere al valore di una proprietà:

```js
// sintassi equivalenti
const proprieta1 = myObject["proprieta1"];

const proprieta2 = myObject.proprieta2;
```

--

## \{Destructuring\}

Estrae una o più proprietà di un oggetto e le assegna a delle variabili

```js
const { inputPath, url, postId, tags, ...postContent } = apiPost;
```

--

## ...Spread

```js
const oldObject = {
  "key1": "first value",
  "key2": "second value",
  "key3": "third value"
};

const myObject = {
  "key0": "zeroth value",
  ...oldObject
};

// > myObject: Object { key0: "zeroth value", key1: "first value", key2: "second value", key3: "third value" }
```

---

## Array

```js
const myArray = ["valore 1", 123, true, "..."];
```

Accedere al valore in una determinata posizione:

```js
const primoElementoDellArray = myArray[0]; // "valore 1"
```

--

## [Destructuring]

Estrae uno o più elementi di un array e assegnarli ad una variabile

```js
const originalArray = ["primo", "secondo", 3];

// destructuring
const [primoElemento, secondoElemento, terzoElemento] = originalArray;

// > primoElemento === "primo"; 
// > secondoElemento === "secondo"; 
// > terzoElemento === 3
```

--

## [Destructuring]

```js
const originalArray = ["primo", "secondo", 3];

// non devi per forza estrarre tutti gli elementi
const [primoElemento, , terzoElemento] = originalArray;

// > primoElemento === "primo"; terzoElemento === 3
```

--

## ...Spread

```js
const myArray = [ 4, 5, 6 ];
const myMergedArray = [ 1, 2, 3, ...myArray ];

// > Array(6) [ 1, 2, 3, 4, 5, 6 ]
```

--

## Metodi degli array

forEach(callback)

```js
myArray.forEach((el) => {
  el = el + "stringa";
});
```

Modifica ogni elemento dell'array, applicando la funzione di callback

```js
myArray === myArray; // true: 
// vengono modificati gli elementi dell'array
```

--

## Metodi degli array

map(callback)

```js
const newArray = myArray.map((el) => {
  el = el + "stringa";
  return el;
})
```

Ritorna un nuovo array, con gli elementi modificati dalla callback

```js
newArray === myArray; // false
// il metodo map() genera un nuovo array
```

--

## Altri metodi degli array

- `Sort:` riordina gli elementi dell’array
- `Find:` trova un elemento
- `Filter`: elimina alcuni elementi
- `Reduce`: elabora tutti gli elementi in un unico risultato
- `Join:` unisce due o più array

--

## Approfondisci

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration)

---

## Funzioni

```js
const miaFunzione = function nomeFunzione() {
  //...
}

const miaArrowFunction = () => {
  //...
}

const funzioneConParametri = (param1, param2) => {
  return param1 + " " + param2;
}
```

--

## First class functions

Si può assegnare una funzione a una variabile

```js
const foo = () => {
  console.log("foobar");
};
foo();
```

--

## First class functions

Si può passare una funzione come argomento di un'altra funzione

```js
function sayHello() {
  return "Hello, ";
}

function greeting(helloMessage, name) {
  console.log(helloMessage() + name);
}

// Pass `sayHello` as an argument to `greeting` function
greeting(sayHello, "JavaScript!");

// Hello, JavaScript!
```

--

## First class functions

Una funzione può ritornare una nuova funzione

```js
function sayHello() {
  return () => {
    console.log("Hello!");
  };
}
```

--

## Approfondisci

[https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function)


--

## Currying

Funzioni che ritornano altre funzioni, con una sintassi fluida

```js
const chiama = (nome) => (functionCognome) => {
  return nome + " " + cognome;
}

// posso chiamarla così:
chiama("Mario")("Bianchi");

// oppure in diversi step,
// assegnando a una variabile la funzione generata:
const chiamaPaolo = chiama("Paolo");
chiamaPaolo("Rossi"); 
```

--

## Approfondisci

[https://it.javascript.info/currying-partials](https://it.javascript.info/currying-partials)

--

## Composizione di funzioni

```js
const famigliaRossi = (nome) => {
  return nome + " Rossi";
}

const famigliaBianchi = (nome) => {
  return nome + " Bianchi";
}

const chiamaConNomeCompleto = (nome, functionCognome) => {
  return functionCognome(nome);
}

chiamaConNomeCompleto("Mario", famigliaBianchi);
chiamaConNomeCompleto("Paolo", famigliaRossi);
```

---

## Funzioni asincrone

--

```js {data-trim data-line-numbers="|13|19|14|15|16|20|7-11|21|1-5|22"}
function salutamiDopo() {
  setTimeout(() => {
    console.log("Ciao");
  }, 1000);
}

const chiamaDopo = (nome) => {
  setTimeout(() => {
    console.log("Ehi, " + nome + "!");
  }, 0); // timeout = zero
}

console.log("Inizio");
salutamiDopo();
chiamaDopo("Mario");
console.log("Fine");

// Console:
// Inizio
// Fine
// Ehi, Mario!
// Ciao
```

--

## Approfondisci

/i/https://youtu.be/N0Au8yc5IOw?si=T0TvfQ-4fFJJL1Gi

[JavaScript Event Loop](https://youtu.be/N0Au8yc5IOw?si=T0TvfQ-4fFJJL1Gi)

--

## Come posso controllare il flusso di una sequenza di funzioni asincrone?

--

## Callback

```js {data-trim data-line-numbers="|14|1-6|17|4|8-12|18"}
function saluta(cb) {
  setTimeout(() => {
    console.log("Ciao");
    cb(); // <- esegue la callback
  }, 1000);
}

const chiama = (nome) => {
  setTimeout(() => {
    console.log("Ehi, " + nome + "!");
  }, 0);
}

saluta(() => chiama("Paolo"));

// Console:
// Ciao
// Ehi, Paolo!
```

--

### Attenzione

La funzione `chiama()` si aspetta un parametro.

Poiché la funzione `saluta()` non passa nessun parametro alla callback,  
`chiama("Paolo")` viene eseguita in una callback che non ha parametri!

```js
saluta((/* nessun parametro */) => chiama("Paolo"));
```

--

## Promise

```js {data-trim data-line-numbers="|19|2|5|11|14"}
function saluta() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("Ciao");
      resolve();
    }, 1000);
  });
}

const chiama = (nome) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("Ehi, " + nome + "!");
      resolve();
    }, 0); 
  });
}

saluta().then(() => chiama("Paolo"));
```

--

## Approfondisci

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

--

## Async/await

```js {data-trim data-line-numbers="|26|19|20|21|1-8|4|5|22|10-17|13|14|23"}
async function saluta() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("Ciao");
      resolve();
    }, 1000);
  });
}

const chiama = async (nome) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("Ehi, " + nome + "!");
      resolve();
    }, 0); 
  });
}

const main = async () => {
  console.log("Inizio");
  await saluta();
  await chiama("Paolo");
  console.log("Fine");
}

main();
```

---

## Mapping con funzioni asincrone

--

Problema: abbiamo un array da mappare su una funzione asincrona

```js
const result = [1, 2, 3, 4].map(async (id) => {
  return await getById(id);
})
```

--

Questa funzione ritorna:

```js
[Promise, Promise, Promise, Promise]
```

mentre ci aspettavamo:

```js
["valuea", "value2", "value3", "value4"]
```

--

Soluzione: `Promise.all()`

```js
const result = Promise.all(
  [1, 2, 3, 4].map(async (id) => {
    return await getById(id);
  })
);
```

--

[Approfondisci su MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)