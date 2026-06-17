## Install on Windows

### Step 1: Download Python

```bash
scoop install python
```

### Step 2: Verify Installation

```bash
python --version
```

## Install on MacOS

### Step 1: Download Python

```bash
brew install python3
```

### Step 2: Verify Installation

```bash
python3 --version
```

> Add `alias python='python3'` to `~/.zshrc`

---

## Install Formatting Tools
Install industry-standard tools to keep your code clean and consistent:
```bash
pip install --upgrade pip
pip install black isort
```
- **[Black](https://black.readthedocs.io/)**: The uncompromising Python code formatter.
- **[isort](https://pycqa.github.io/isort/)**: A utility to sort your Python imports alphabetically and automatically separated into sections.

---

## 💻 IDE Configuration: Visual Studio Code

1. Download and install [Visual Studio Code](https://code.visualstudio.com/).
2. Open your project folder in VS Code (`File > Open Folder`).
3. Open the Extensions view (`Ctrl+Shift+X` or `Cmd+Shift+X`) and install the following:

| Extension | Publisher | Purpose |
| :--- | :--- | :--- |
| **Python** | Microsoft | Core Python language support, debugging, and REPL. |
| **Pylance** | Microsoft | Fast, feature-rich language support (type checking, IntelliSense). |
| **Black Formatter** | Microsoft | Official extension to format code on save using Black. |
| **isort** | Microsoft | Automatically sorts and organizes import statements. |

### Configure VS Code Settings
Add the following to your `.vscode/settings.json` (or user settings) to format code automatically when you save:

```json
{
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.organizeImports": "explicit"
    }
  }
}
```
