# Getting Started with DevTools

DevTools (Developer Tools) is a set of debugging tools built into your browser. Nothing to install — every modern browser (Chrome, Edge, Firefox, Safari, etc.) ships with them.

They help you answer three of the most common development questions:

- Why does this element look wrong?
- Why is this JavaScript throwing an error?
- Why is this API request failing?

This guide covers only the most commonly used panels. The goal is to help you know *where to look*, not to teach you everything.

## 1. How to Open DevTools

The most common ways:

- **macOS**: `Cmd + Option + I`
- **Windows / Linux**: `F12`, or `Ctrl + Shift + I`
- **Right-click → Inspect**: Right-click any element on the page and choose "Inspect" — it jumps straight to that element
- **Toggle device (mobile) view**: `Cmd + Shift + M` (macOS), or `Ctrl + Shift + M` (Windows / Linux)

Once open, the interface is usually split into a few sections. All the panels below are available as tabs at the top.

## 2. Elements

**Where**: The first tab in DevTools.

What it does:

- **View HTML / DOM**: The structure the page actually renders — not always identical to the source code
- **View CSS**: Select an element to see all styles applied to it in the right-hand pane
- **Edit CSS**: Double-click a style value to change it — the page updates live, great for quick experimentation
- **Inspect dimensions**: Selecting an element shows its width / height / margin / padding visually

**When to use it**: When page styles look wrong or the layout is off (misaligned, odd spacing, not centered).

Note: changes you make in Elements disappear on refresh — once you find a fix, copy it back into your code.

## 3. Console

**Where**: The "Console" tab in DevTools.

What it does:

- **View JavaScript errors**: Red error messages appear here, including the file and line number
- **Use `console.log()`**: Print variables from your code — the most common debugging technique
- **Run JavaScript directly**: Type an expression and press Enter, e.g. `document.title`

**When to use it**: When JavaScript errors occur or the code behaves unexpectedly.

Build a habit: **whenever something is wrong with a page, check the Console first**. Many "the page doesn't work" moments turn out to be a single red error line here.

## 4. Network

**Where**: The "Network" tab in DevTools.

What it does: shows every HTTP request the browser makes. Each request includes:

- **URL**: where the request was sent
- **Method**: the request type (GET, POST, etc.)
- **Status**: the HTTP status code — whether the request succeeded or failed
- **Request / Response**: the parameters sent and the data returned by the server

**When to use it**: When an API request fails or the response looks wrong.

Open the panel and refresh the page to see all requests. Click any one of them for details.

### Common Status Codes

| Status Code | Meaning |
| --- | --- |
| `200` | OK |
| `400` | Bad Request — the request parameters are wrong |
| `401` | Unauthorized — not logged in or session expired |
| `403` | Forbidden — no permission |
| `404` | Not Found — the resource doesn't exist |
| `500` | Internal Server Error — something broke on the server |

When debugging an API issue, check the status code first, then look at the error message in the Response.

## 5. Sources

**Where**: The "Sources" tab in DevTools.

What it does:

- **View JavaScript source code**: Browse every script the page loads, organized by file
- **Breakpoint**: Click a line number to pause execution when the code reaches that line
- **Step through code**: Once paused, run the code line by line, watching how variables change — often alongside `console.log`

**When to use it**: When the JavaScript logic is wrong and you need to trace through the execution step by step.

Breakpoints are better than scattering `console.log` everywhere for figuring out *why the code took a certain path* — but at the beginner stage, `console.log` alone is enough.

## 6. A Simple Debugging Workflow

When something goes wrong on a page, don't panic — follow this order:

```text
Something is wrong with the page
    ↓
Check the Console first
    ↓
Style issue        → Elements
API/request issue  → Network
Logic issue        → Sources
```
