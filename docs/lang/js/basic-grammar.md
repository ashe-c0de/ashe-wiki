## print

```hello.js
console.log("Hello, World!");
console.error("Something went wrong");
console.warn("Warning");
console.info("Information");
```

node hello.js


## variables
```
const name = "Ashe";
let age = 20;

age = 21;
```
## types

```
const name = "Charles";       // string
const age = 20;               // number
const online = true;          // boolean
const value = undefined;      // undefined
const empty = null;           // null

const user = { name: "Charles" }; // object
const numbers = [1, 2, 3];        // array
```

```mermaid
graph TD
    JS[JavaScript Data Types]

    JS --> Prim[Primitive Types]
    JS --> Obj[Objects / Reference Types]

    Prim --> P1[String]
    Prim --> P2[Number]
    Prim --> P3[BigInt]
    Prim --> P4[Boolean]
    Prim --> P5[Undefined]
    Prim --> P6[Null]
    Prim --> P7[Symbol]

    Obj --> O1[Standard Object]
    Obj --> O2[Array]
    Obj --> O3[Function]
    Obj --> O4[Built-in Objects]

    O4 --> B1[Date]
    O4 --> B2[RegExp]
    O4 --> B3[Map / Set]
    O4 --> B4[Error]
    O4 --> B5[Promise]
```

## functions

## arrays

## objects