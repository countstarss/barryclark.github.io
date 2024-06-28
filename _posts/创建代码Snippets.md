# 创建代码Snippets

在 Visual Studio Code (VSCode) 中创建代码片段（Snippets）是一个非常有用的功能，可以帮助你快速插入常用的代码块。以下是如何创建和使用代码片段的详细步骤：

### 创建代码片段

1. **打开用户片段配置文件**：
    - 打开 VSCode。
    - 按 `Ctrl + Shift + P`（Windows/Linux）或 `Cmd + Shift + P`（macOS）打开命令面板。
    - 输入 `Preferences: Configure User Snippets` 并选择它。
    - 选择你想要创建片段的语言（例如，JavaScript，HTML，等）。你也可以选择 `New Global Snippets file` 来创建全局片段。

2. **添加代码片段**：
    - 选择语言后，VSCode 会打开一个片段配置文件（`.json` 文件）。
    - 在这个文件中，你可以添加你的代码片段。以下是一个示例：

```json
{
  // Place your snippets for javascript here. Each snippet is defined under a snippet name and has a prefix, body and
  // description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
  // $1, $2 for tab stops, $0 for the final cursor position, and ${1:default} for placeholders. Example:
  "Print to console": {
    "prefix": "log",
    "body": [
      "console.log('$1');",
      "$2"
    ],
    "description": "Log output to console"
  }
}
```

### 代码片段示例

以下是一些常用的代码片段示例：

#### JavaScript Console Log

```json
{
  "Print to console": {
    "prefix": "log",
    "body": [
      "console.log('$1');",
      "$2"
    ],
    "description": "Log output to console"
  }
}
```

#### HTML Boilerplate

```json
{
  "HTML Boilerplate": {
    "prefix": "html5",
    "body": [
      "<!DOCTYPE html>",
      "<html lang=\"en\">",
      "<head>",
      "    <meta charset=\"UTF-8\">",
      "    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">",
      "    <title>${1:Document}</title>",
      "</head>",
      "<body>",
      "    $2",
      "</body>",
      "</html>"
    ],
    "description": "HTML5 Boilerplate"
  }
}
```

### 代码片段的结构

- **prefix**：触发片段的关键字。你在代码中输入这个前缀并按 `Tab` 键时，会插入相应的代码片段。
- **body**：这是一个数组，每一行表示插入代码中的一行。你可以使用 `$1`, `$2` 等占位符来创建光标位置。
- **description**：对片段的简要描述。

### 使用代码片段

1. **输入前缀**：
    - 在编辑器中，输入你设置的前缀（例如 `log` 或 `html5`）。
2. **触发代码片段**：
    - 输入前缀后，按 `Tab` 键。VSCode 会用你定义的代码片段替换前缀。
3. **填充占位符**：
    - 如果你在代码片段中使用了占位符（例如 `$1`, `$2`），光标会自动跳到这些位置，让你可以快速填充内容。

### 示例

假设你在 JavaScript 中定义了一个 `console.log` 的片段：

```json
{
  "Print to console": {
    "prefix": "log",
    "body": [
      "console.log('$1');",
      "$2"
    ],
    "description": "Log output to console"
  }
}
```

使用方法：

1. 在 JavaScript 文件中输入 `log`。
2. 按 `Tab` 键，会自动插入 `console.log('');`，光标会跳到引号内，方便你输入日志内容。

通过上述步骤，你可以在 VSCode 中轻松创建和使用代码片段，提高编码效率和速度。