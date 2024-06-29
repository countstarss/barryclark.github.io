
## 添加代码标记
  - `// TODO: 任务`
  - `// FIXME: 问题`

## 打开命令面板
  - `Ctrl + Shift + P`（Windows/Linux）
  - `Cmd + Shift + P`（Mac）

在Visual Studio Code中，使用TODO Highlight插件的步骤如下：

1. **安装TODO Highlight插件**：
   - 打开VSCode。
   - 点击左侧的扩展图标（四个方块的图标）。
   - 在搜索框中输入“TODO Highlight”。
   - 找到并安装插件。

2. **使用TODO Highlight**：
   - 打开一个代码文件。
   - 在代码中添加标记，例如 `// TODO: 任务` 或 `// FIXME: 问题`。
   - 插件会自动高亮这些标记，使其在代码中更加显眼。

3. **查看所有标记**：
   - 按 `Ctrl + Shift + P`（Windows/Linux）或 `Cmd + Shift + P`（Mac），打开命令面板。
   - 输入并选择“TODO Highlight: List Highlighted Annotations”。
   - 一个列表会显示所有高亮的标记，点击某个标记即可跳转到对应位置。

4. **自定义配置**：
   - 可以在 `settings.json` 中自定义高亮的关键词和颜色。
   - 打开设置文件，添加如下配置：
     ```json
     "todohighlight.keywords": [
       {
         "text": "TODO",
         "color": "#FF0000",
         "backgroundColor": "#FFFF00",
       },
       {
         "text": "FIXME",
         "color": "#FFFFFF",
         "backgroundColor": "#FF0000",
       }
     ]
     ```

通过这些步骤，你可以在VSCode中使用TODO Highlight插件来标记和管理代码中的待办事项和问题。