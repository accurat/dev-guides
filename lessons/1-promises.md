# Promises

Le promises sono ...

## Esempio: richiesta a un endpoint

Ad esempio, uno dei casi più frequenti è:

```js
fetch('/asd.txt')
  .then((res) => {
    return res.text()
  })
  .then((text) => {
    console.log(text)
  })
```

Dove:

```ts
fetch(url: string): Promise<Response>
res: Response
res.text(): Promise<string>
text: string
```

Ma come facciamo ad utilizzare `text` nella nostra applicazione?

App:
```js
function app(name) {
  document.body.innerHTML = `Welcome, dear ${name}!`
}

app('user')
```

La feature richiesta è sostutuire la parola "user" con il nome utente, 
contenuto nel file `/name.txt`, che va ottenuto con richiesta asincrona.

Soluzione ERRATA:

```js
function app(name) {
  document.body.innerHTML = `Welcome, dear ${name}!`
}

document.body.innerHTML = 'LOADING...'

let name

fetch('/name.txt')
  .then((res) => {
    return res.text()
  })
  .then((text) => {
    name = text
  })
  
app(name)
```


Soluzione corretta:

```js
function app(name) {
  document.body.innerHTML = `Welcome, dear ${name}!`
}

document.body.innerHTML = 'LOADING...'

fetch('/name.txt')
  .then((res) => {
    return res.text()
  })
  .then((text) => {
    app(text)
  })
```

## Promises e Callbacks

### cosa sono le Callbacks

TODO

### differenza Callback - Promise, conversione

TODO

### `setTimeout` con Promise

```js
function wait(ms, val) {
  return new Promise((resolve) => {
    setTimeout(() => resolve(val), ms)
  })
}

function fetchXHR() {
  return new Promise((resolve) => {
    let xhr = new XMLHttpRequest()
    xhr.open('GET', '/article/xmlhttprequest/example/load')
    xhr.send()
    xhr.onload = () => resolve(xhr.response)
  })
}
```

### callbacks in casi avanzati

```js
function sendXHR(url, callback) {
  let xhr = new XMLHttpRequest()
  xhr.open('GET', url)
  xhr.send()
  xhr.onload = () => callback(xhr.response)
}

const files = ['a.txt', 'b.txt']

let responsesCount = 0
const responses = []

files.forEach((f, i) => {
  sendXHR(f, (res) => {
    responses[i] = res
    responsesCount++

    if (responsesCount === files.length) {
      console.log(responses)
    }
  })
})
```

Lo stesso esempio ma usando le Promises:

```js
// Promise.all(Array<Promise<T>): Promise<Array<T>>
// responses: Array<string>

Promise.all(
  files.map((f, i) => {
    return fetch(f).then((r) => r.text())
  }),
).then((responses) => {
  console.log(responses)
})
```


## async/await

Questo costrutto ci da l'illusione di lavorare in modo sincrono, è infatti solo un **syntactic sugar** per rendere più facile l'uso di valori asincroni.

Rifacciamo lo stesso esempio iniziale ma con `async`/`await`:

```js
function app(name) {
  document.body.innerHTML = `Welcome, dear ${name}!`
}

async function fetchEndpoint() {
  const response = await fetch('/name.txt')
  const text = await response.text()
  return text
}

document.body.innerHTML = 'LOADING...'

fetchEndpoint().then(app)
```

Ricordarsi che dietro le quinte è sempre asincrono, e dietro le quinte viene usato `then`!

## ESERCIZI

1. fetch `1.txt` and use its content as URL to fetch the next endpoint (file `1.txt` has content `"2.txt"`, file `2.txt` has content `"surprise!"`)
2. create an example using `Promise.race()` or `Promise.any()`
3. try using promises in React to retrieve a dataset (call the endpoint in an `useEffect`, and save its output in an `useState`)
4. fetch an array of 3 files, and output a single object with the file names as keys and the contents as values
5. did you fetch the files sequentially or parallelly? try doing it in the other way! sequentially = using a chain of `then`, parallelly = using `Promise.all()`. what is the best method?
6. re-do all excercises 1-5 using `async`/`await`
