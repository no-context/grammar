grammar (WIP)
=======

Define context-free grammars for use with parsing tools like [nearley](https://github.com/Hardmath123/nearley).

    $ npm install grammar

Use template strings to define your grammar.

```js
const bnf = require('grammar')

const id = ([x]) => x
const select = n => d => d[n]
const newObject = () => ({})
const newArray = () => []

const jsonGrammar = bnf`
  json -> object  ${id}
        | array   ${id}

  object  -> "{" members "}" ${select(1)}
  members -> null         ${newObject}
           | members pair ${([obj, pair]) => Object.assign(obj, pair)}
  pair -> string ":" value  ${([key, _, value]) => ({[key]: value})}

  array -> "[" items "]"  ${select(1)}
  items -> null         ${newArray}
         | items value  ${([xs, x]) => {xs.push(x); return xs}}

  value -> object   ${id}
         | array    ${id}
         | number   ${id}
         | string   ${id}
         | "true"   ${() => true}
         | "false"  ${() => false}
         | "null"   ${() => null}

  number -> %NUMBER  ${([tok]) => parseFloat(tok.value) }}
  string -> %STRING  ${([tok]) => JSON.parse(tok.value) }}
`
```

Now you can use it!

```js
const nearley = require('nearley')
const p = nearley.Parser(jsonGrammar)
/// ...
```
