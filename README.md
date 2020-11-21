# Linee Guida per il Frontend

## HTML

### Semantica

HTML5 fornisce molti elementi semantici atti a descrivere con precisione il contenuto. Ci si assicuri di beneficiare del suo ricco vocabolario.

```html
<!-- male -->
<div id=main>
  <div class=article>
    <div class=header>
      <h1>Blog post</h1>
      <p>Published: <span>21st Feb, 2015</span></p>
    </div>
    <p>…</p>
  </div>
</div>

<!-- bene -->
<main>
  <article>
    <header>
      <h1>Blog post</h1>
      <p>Published: <time datetime=2015-02-21>21st Feb, 2015</time></p>
    </header>
    <p>…</p>
  </article>
</main>
```

Ci si assicuri di comprendere la semantica degli elementi che si stanno utilizzando. E' peggio utilizzare un elemento semantico nel modo sbagliato che rimanere neutrali.

```html
<!-- male -->
<h1>
  <figure>
    <img alt=Company src=logo.png>
  </figure>
</h1>

<!-- bene -->
<h1>
  <img alt=Company src=logo.png>
</h1>
```

### Concisione

Mantenere il proprio codice conciso. Dimenticare le vecchie abitudini di XHTML.

```html
<!-- male -->
<!doctype html>
<html lang=en>
  <head>
    <meta http-equiv=Content-Type content="text/html; charset=utf-8" />
    <title>Contact</title>
    <link rel=stylesheet href=style.css type=text/css />
  </head>
  <body>
    <h1>Contact me</h1>
    <label>
      Email address:
      <input type=email placeholder=you@email.com required=required />
    </label>
    <script src=main.js type=text/javascript></script>
  </body>
</html>

<!-- bene -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Contact</title>
  <link rel=stylesheet href=style.css>

  <h1>Contact me</h1>
  <label>
    Email address:
    <input type=email placeholder=you@email.com required>
  </label>
  <script src=main.js></script>
</html>
```

### Accessibilità

La cura dell'accessibilità non dovrebbe essere un'aggiunta. Non occorre essere un esperto WCAG per migliorare il proprio sito web, si può partire subito sistemando quelle piccole cose che fanno una grande differenza tipo:

* si impari a utilizzare propriamente l'attributo `alt` 
* ci si assicuri che i propri collegamenti e bottoni siano marcati come tali (non atrocità tipo  `<div class=button>` )
* non ci si affidi esclusivamente sui colori per comunicare informazioni
* si etichettino esplicitamente i controlli nei form

```html
<!-- male -->
<h1><img alt=Logo src=logo.png></h1>

<!-- bene -->
<h1><img alt=Company src=logo.png></h1>
```

### Linguaggio e codifica caratteri

Definire un linguaggio è opzionale, ma è raccomandato dichiararlo sempre nell'elemento radice.

HTML standard richiede che le pagine utilizzino una codifica di carattere UTF-8.
Deve essere dichiarato, sebbene possa essere dichiarato nll'intestazione HTTP Content-Type, è raccomandato dichiarlo sempre a livello di documento.

```html
<!-- male -->
<!doctype html>
<title>Hello, world.</title>

<!-- bene -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Hello, world.</title>
</html>
```

### Prestazione

A meno che ci sia una valida ragione per caricare i propri script prima del contenuto, non si blocchi iUnless there's a valid reason for loading your scripts before your content, non si blocchi l'interpretazione della pagina. Se il proprio foglio di stile è pesante, si isolino gli stili che sono assolutamente richiesti all'inizio e si differisca il caricamento delle dichiarazioni secondarie in un foglio di stile separato. Due richieste HTTP sono significativamente più lente di una sola, ma la percezione di velocità è il fattore più importante..

```html
<!-- male -->
<!doctype html>
<meta charset=utf-8>
<script src=analytics.js></script>
<title>Hello, world.</title>
<p>...</p>

<!-- bene -->
<!doctype html>
<meta charset=utf-8>
<title>Hello, world.</title>
<p>...</p>
<script src=analytics.js></script>
```

## CSS

### Punti e Virogla

Mentre il punto e virgola è tecnicamente un separatore in CSS, lo si tratti sempre come un terminatore.

```css
/* bad */
div {
  color: red
}

/* good */
div {
  color: red;
}
```

### Il modello Box

Il modello box dovrebbe idealmente essere lo stesso per l'intero documento. Un 
`* { box-sizing: border-box; }` globale va bene, ma non si modifichi il modello box predefinito su specifici elementi se si può evitare.

```css
/* bad */
div {
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
}

/* good */
div {
  padding: 10px;
}
```

### Flusso

Non si modifichi il comportamento predefinito di un elemento se si può evitare. Si mantengano gli elementi nel normale flusso del documento il più possibile. Per esempio, la rimozione di una spaziatura sotto un'immagine non dovrebbe costringere a cambiare la sua visualizzazione predefinita:

```css
/* bad */
img {
  display: block;
}

/* good */
img {
  vertical-align: middle;
}
```

Alla stessa stregua, non si tolga un elemento dal flusso se si può evitare.

```css
/* bad */
div {
  width: 100px;
  position: absolute;
  right: 0;
}

/* good */
div {
  width: 100px;
  margin-left: auto;
}
```

### Posizionamento

Ci sono molti modi per posizionare un alemento in CSS. Si preferiscano specifiche di disposizione moderne tipo  Flexbox e  Grid, e si eviti di rimuovere elementi dal normale flusso del documento, per esempio con 
`position: absolute`.

### Selettori

Si minimizzino i selettori fortemente accoppiati con il DOM. Si consideri l'aggiunta di una classe agli elementi che si vogliono far corrispondere quando il proprio selettore supera le 3 pseudo-classi strutturali, o combinazioni discendenti o alle stesso livello.

```css
/* bad */
div:first-of-type :last-child > p ~ *

/* good */
div:first-of-type .info
```

Si eviti il sovraccarico dei proprio selettori quando non necessario.

```css
/* bad */
img[src$=svg], ul > li:first-child {
  opacity: 0;
}

/* good */
[src$=svg], ul > :first-child {
  opacity: 0;
}
```

### Specificità

Non si rendano valori e selettori difficili da sovrascrivere. Si minimizzi l'uso di  `id`'s
e si eviti `!important`.

```css
/* bad */
.bar {
  color: green !important;
}
.foo {
  color: red;
}

/* good */
.foo.bar {
  color: green;
}
.foo {
  color: red;
}
```

### Sovrascrittura

La sovrascrittura degli stili rende i selettori e il debug più difficile. Si eviti quanto possibile.

```css
/* bad */
li {
  visibility: hidden;
}
li:first-child {
  visibility: visible;
}

/* good */
li + li {
  visibility: hidden;
}
```

### Ereditarietà

Non si duplichino le dichiarazioni di stile che possono essere ereditate.

```css
/* bad */
div h1, div p {
  text-shadow: 0 1px 0 #fff;
}

/* good */
div {
  text-shadow: 0 1px 0 #fff;
}
```

### Brevità

Si mantenga il proprio codice conciso. Si usino scorciatoie di proprietà e si eviti di usare proprietà multiple quando non necessario.

```css
/* bad */
div {
  transition: all 1s;
  top: 50%;
  margin-top: -10px;
  padding-top: 5px;
  padding-right: 10px;
  padding-bottom: 20px;
  padding-left: 10px;
}

/* good */
div {
  transition: 1s;
  top: calc(50% - 10px);
  padding: 5px 10px 20px;
}
```

### Language

Si preferisce l'inglese al posto della matematica.

```css
/* bad */
:nth-child(2n + 1) {
  transform: rotate(360deg);
}

/* good */
:nth-child(odd) {
  transform: rotate(1turn);
}
```

### Prefissi dei Venditori

Ci si sbarazzi aggressivamente dei prefissi dei fornitori. Se si devono usare, si inseriscano prima delle proprietà standard.

```css
/* bad */
div {
  transform: scale(2);
  -webkit-transform: scale(2);
  -moz-transform: scale(2);
  -ms-transform: scale(2);
  transition: 1s;
  -webkit-transition: 1s;
  -moz-transition: 1s;
  -ms-transition: 1s;
}

/* good */
div {
  -webkit-transform: scale(2);
  transform: scale(2);
  transition: 1s;
}
```

### Animazioni

Si preferiscano le transizioni rispetto alle animazioni. Si eviti di anomare proprietà diverse da 
`opacity` e `transform`.

```css
/* bad */
div:hover {
  animation: move 1s forwards;
}
@keyframes move {
  100% {
    margin-left: 100px;
  }
}

/* good */
div:hover {
  transition: 1s;
  transform: translateX(100px);
}
```

### Unità

Si usino valori che rappresentano valori di unità quando possibile. Si preferisca`rem` se si usano unità relative. Si preferiscano i secondi rispetto ai millisecondi.

```css
/* bad */
div {
  margin: 0px;
  font-size: .9em;
  line-height: 22px;
  transition: 500ms;
}

/* good */
div {
  margin: 0;
  font-size: .9rem;
  line-height: 1.5;
  transition: .5s;
}
```

### Colori

Se serve trasparenza, si usi `rgba`. Altrimenti si usi sempre il formato esadecimale.

```css
/* bad */
div {
  color: hsl(103, 54%, 43%);
}

/* good */
div {
  color: #5a3;
}
```

### Disegno

Si evitino le richieste HTTP quando le risorse sono facilmente replicabili con CSS.

```css
/* bad */
div::before {
  content: url(white-circle.svg);
}

/* good */
div::before {
  content: "";
  display: block;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #fff;
}
```

### Forzature

Non si usino.

```css
/* bad */
div {
  // position: relative;
  transform: translateZ(0);
}

/* good */
div {
  /* position: relative; */
  will-change: transform;
}
```

## JavaScript

### Prestazioni

Si preferiscano leggibilità, correttezza ed espressività rispetto alle prestazioni. Javascript non sarà praticamente mai il collo di bottiglia delle proprie prestazioni. Si ottimizzino cose tipo la compressione immagine, l'accesso alla rete e il riflusso del DOM. Se ci si deve ricordare solo una linea guida di questo documento, si scelga questa.

```javascript
// bad (albeit way faster)
const arr = [1, 2, 3, 4];
const len = arr.length;
var i = -1;
var result = [];
while (++i < len) {
  var n = arr[i];
  if (n % 2 > 0) continue;
  result.push(n * n);
}

// good
const arr = [1, 2, 3, 4];
const isEven = n => n % 2 == 0;
const square = n => n * n;

const result = arr.filter(isEven).map(square);
```

### Mancanza di Stato

Si cerchi di mantenere le proprie funzioni pure. Tutte le funzioni idalmente non dovrebbero produrre effetti collaterali, non usare dati esterni e ritornare nuovi oggeti invece di modificare quelli esistenti.

```javascript
// bad
const merge = (target, ...sources) => Object.assign(target, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }

// good
const merge = (...sources) => Object.assign({}, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }
```

### Nativi

Affidarsi ai metodi nativi il più possibile.

```javascript
// bad
const toArray = obj => [].slice.call(obj);

// good
const toArray = (() =>
  Array.from ? Array.from : obj => [].slice.call(obj)
)();
```

### Coercizione

Adottare la coercizione implicita quando ha senso. Si eviti altrimenti.

```javascript
// bad
if (x === undefined || x === null) { ... }

// good
if (x == undefined) { ... }
```

### Cicli

Non si usino cicli che forzano l'utilizzo di oggetti mutabili. Si faccia affidamento ai metodi  `array.prototype` 

```javascript
// bad
const sum = arr => {
  var sum = 0;
  var i = -1;
  for (;arr[++i];) {
    sum += arr[i];
  }
  return sum;
};

sum([1, 2, 3]); // => 6

// good
const sum = arr =>
  arr.reduce((x, y) => x + y);

sum([1, 2, 3]); // => 6
```
Se non è possibile oppure usando  `array.prototype` non ha logicamente senso, si usi la ricorsione.

```javascript
// bad
const createDivs = howMany => {
  while (howMany--) {
    document.body.insertAdjacentHTML("beforeend", "<div></div>");
  }
};
createDivs(5);

// bad
const createDivs = howMany =>
  [...Array(howMany)].forEach(() =>
    document.body.insertAdjacentHTML("beforeend", "<div></div>")
  );
createDivs(5);

// good
const createDivs = howMany => {
  if (!howMany) return;
  document.body.insertAdjacentHTML("beforeend", "<div></div>");
  return createDivs(howMany - 1);
};
createDivs(5);
```

Ecco una [funzione generica di ciclo](https://gist.github.com/bendc/6cb2db4a44ec30208e86) che facilita l'utilizzo della ricorsione.

### Argumenti

Dimenticarsi dell'oggetto `arguments`. Il parametro rest è sempre una migliore opzione in quanto:

1. è denominato, quindi fornisce una migliore idea di quali argomenti una funzione si aspetti
2. è un vero array, il che ne facilita l'uso.

```javascript
// bad
const sortNumbers = () =>
  Array.prototype.slice.call(arguments).sort();

// good
const sortNumbers = (...numbers) => numbers.sort();
```

### Apply

Forget about `apply()`. Use the spread operator instead.

```javascript
const greet = (first, last) => `Hi ${first} ${last}`;
const person = ["John", "Doe"];

// bad
greet.apply(null, person);

// good
greet(...person);
```

### Bind

Don't `bind()` when there's a more idiomatic approach.

```javascript
// bad
["foo", "bar"].forEach(func.bind(this));

// good
["foo", "bar"].forEach(func, this);
```
```javascript
// bad
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = function() {
      return `${this.first} ${this.last}`;
    }.bind(this);
    return `Hello ${full()}`;
  }
}

// good
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = () => `${this.first} ${this.last}`;
    return `Hello ${full()}`;
  }
}
```

### Higher-order functions

Avoid nesting functions when you don't have to.

```javascript
// bad
[1, 2, 3].map(num => String(num));

// good
[1, 2, 3].map(String);
```

### Composition

Avoid multiple nested function calls. Use composition instead.

```javascript
const plus1 = a => a + 1;
const mult2 = a => a * 2;

// bad
mult2(plus1(5)); // => 12

// good
const pipeline = (...funcs) => val => funcs.reduce((a, b) => b(a), val);
const addThenMult = pipeline(plus1, mult2);
addThenMult(5); // => 12
```

### Caching

Cache feature tests, large data structures and any expensive operation.

```javascript
// bad
const contains = (arr, value) =>
  Array.prototype.includes
    ? arr.includes(value)
    : arr.some(el => el === value);
contains(["foo", "bar"], "baz"); // => false

// good
const contains = (() =>
  Array.prototype.includes
    ? (arr, value) => arr.includes(value)
    : (arr, value) => arr.some(el => el === value)
)();
contains(["foo", "bar"], "baz"); // => false
```

### Variables

Favor `const` over `let` and `let` over `var`.

```javascript
// bad
var me = new Map();
me.set("name", "Ben").set("country", "Belgium");

// good
const me = new Map();
me.set("name", "Ben").set("country", "Belgium");
```

### Conditions

Favor IIFE's and return statements over if, else if, else and switch statements.

```javascript
// bad
var grade;
if (result < 50)
  grade = "bad";
else if (result < 90)
  grade = "good";
else
  grade = "excellent";

// good
const grade = (() => {
  if (result < 50)
    return "bad";
  if (result < 90)
    return "good";
  return "excellent";
})();
```

### Object iteration

Avoid `for...in` when you can.

```javascript
const shared = { foo: "foo" };
const obj = Object.create(shared, {
  bar: {
    value: "bar",
    enumerable: true
  }
});

// bad
for (var prop in obj) {
  if (obj.hasOwnProperty(prop))
    console.log(prop);
}

// good
Object.keys(obj).forEach(prop => console.log(prop));
```

### Objects as Maps

While objects have legitimate use cases, maps are usually a better, more powerful choice. When in
doubt, use a `Map`.

```javascript
// bad
const me = {
  name: "Ben",
  age: 30
};
var meSize = Object.keys(me).length;
meSize; // => 2
me.country = "Belgium";
meSize++;
meSize; // => 3

// good
const me = new Map();
me.set("name", "Ben");
me.set("age", 30);
me.size; // => 2
me.set("country", "Belgium");
me.size; // => 3
```

### Curry

Currying is a powerful but foreign paradigm for many developers. Don't abuse it as its appropriate
use cases are fairly unusual.

```javascript
// bad
const sum = a => b => a + b;
sum(5)(3); // => 8

// good
const sum = (a, b) => a + b;
sum(5, 3); // => 8
```

### Readability

Don't obfuscate the intent of your code by using seemingly smart tricks.

```javascript
// bad
foo || doSomething();

// good
if (!foo) doSomething();
```
```javascript
// bad
void function() { /* IIFE */ }();

// good
(function() { /* IIFE */ }());
```
```javascript
// bad
const n = ~~3.14;

// good
const n = Math.floor(3.14);
```

### Code reuse

Don't be afraid of creating lots of small, highly composable and reusable functions.

```javascript
// bad
arr[arr.length - 1];

// good
const first = arr => arr[0];
const last = arr => first(arr.slice(-1));
last(arr);
```
```javascript
// bad
const product = (a, b) => a * b;
const triple = n => n * 3;

// good
const product = (a, b) => a * b;
const triple = product.bind(null, 3);
```

### Dependencies

Minimize dependencies. Third-party is code you don't know. Don't load an entire library for just a couple of methods easily replicable:

```javascript
// bad
var _ = require("underscore");
_.compact(["foo", 0]));
_.unique(["foo", "foo"]);
_.union(["foo"], ["bar"], ["foo"]);

// good
const compact = arr => arr.filter(el => el);
const unique = arr => [...new Set(arr)];
const union = (...arr) => unique([].concat(...arr));

compact(["foo", 0]);
unique(["foo", "foo"]);
union(["foo"], ["bar"], ["foo"]);
```

*[WCAG]: Web Content Accessibility Guidelines (Linee Guida per Accessibilità di Contenuti Web)