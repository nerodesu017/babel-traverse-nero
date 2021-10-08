# @babel/traverse

> The Babel Traverse module maintains the overall tree state, and is responsible for replacing, removing, and adding nodes

> A single line of code patched by nerodesu017 to offer overall skip of certain nodes


See our website [@babel/traverse](https://babeljs.io/docs/en/babel-traverse) for more information or the [issues](https://github.com/babel/babel/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3A%22pkg%3A%20traverse%22+is%3Aopen) associated with this package.

## Install

Using npm:

```sh
npm install https://github.com/nerodesu017/babel-traverse-nero
```

or using yarn:

```
LMFAOOOOO SIKE YOU THOUGHT

jk i acc dunno how to make u download this on yarn

use git u freak
```

## How to use?

### 1. Find the Node you want to skip (skip itself and, thus, inner Nodes)
### 2. Set its "nero_skip" property to true
### 3. That's it

<br><br>

### **Transform.js**

```js
const fs = require("fs");
const t = require("@babel/types");
const traverse = require("@babel/traverse").default;
const parse = require("@babel/parser").parse;
const generate = require("@babel/generator").default;

let fileName = "in";
let file = fs.readFileSync(fileName + ".js", {"encoding": "utf-8"})

const AST = parse(file);

function genCode(node){
  return generate(node, ...Array.from(arguments).slice(1)).code;
}

// The function we care about most
traverse(AST, {
  FunctionDeclaration(path){
    if(path.node.id.name === "b"){
      path.node.nero_skip = true;
    }
  }
})
// The function we care about most

traverse(AST, {
  Identifier(path){
    // It will only change the first occurence in your case
    if(path.node.name === "a"){
      path.scope.rename("a", "renamed");
    }

    // We won't get to our only "b" identifier occurence
    if(path.node.name === "b"){
      path.scope.rename("b", "thisWillNotBeRenamed");
    }
  }
})

fs.writeFileSync(fileName + "_out.js", genCode(AST));
```

### **in.js**

```js
let a = 3;

function b() {
  let a = 4;
  function c() {
    let a = 5;
  }
}
```

### **in_out.js**

```js
let renamed = 3;

function b() {
  let a = 4;

  function c() {
    let a = 5;
  }
}
```

## Downsides

### I'm not sure how you'd unset the nero_skip property
### I guess you can only generate and parse again, good for my use case
### Submit pull reqs and I'll try and review them