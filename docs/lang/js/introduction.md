JavaScript is a programming language.

A JavaScript engine executes JavaScript. A runtime environment combines the engine with additional APIs and capabilities. Browser and Node.js are two different JavaScript runtime environments.

```mermaid
graph TD
    JS[JavaScript]
    JS --> Browser[Browser]
    JS --> Node[Node.js]

    Browser --> WebAPI[Web APIs]
    Node --> NodeAPI[Node.js APIs]
```