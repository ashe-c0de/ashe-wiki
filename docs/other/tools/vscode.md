# VS Code Quick Start Guide

Visual Studio Code (VS Code) is a lightweight and powerful code editor. Here is a simplified guide to get you up and running quickly.

---

## 1. Essential Keyboard Shortcuts

| Action                  | macOS             | Windows / Linux    |
| :---------------------- | :---------------- | :----------------- |
| **Command Palette**     | `Cmd + Shift + P` | `Ctrl + Shift + P` |
| **Quick Open File**     | `Cmd + P`         | `Ctrl + P`         |
| **Toggle Terminal**     | ``Ctrl + ` ``     | ``Ctrl + ` ``      |
| **Toggle Sidebar**      | `Cmd + B`         | `Ctrl + B`         |
| **Multi-Cursor Select** | `Cmd + D`         | `Ctrl + D`         |

---

## 2. Essential Settings (`settings.json`)

Open the Command Palette (`Cmd/Ctrl + Shift + P`), search for **Preferences: Open User Settings (JSON)**, and paste:

```json
{
  "editor.fontSize": 16,
  "editor.tabSize": 2,
  "editor.fontFamily": "'Fira Code', 'JetBrains Mono', Consolas, monospace",
  "editor.formatOnSave": true,
  "files.autoSave": "onFocusChange"
}
```

## 3. Extensions (for React?)

- ESLint
- Error Lens
- Prettier - Code formatter
- GitLens
