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

### differenza Callback - Promise, conversione

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

const files = ['asd.txt', 'qwe.txt']

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

```js
async function asd() {
  const response = await fetch('/asd.txt')
  console.log(response)
  const text = await response.text()
  await wait(1000)
  return response.status + text
}

asd().then(console.log)
```
