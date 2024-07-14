- [ ] show all commands

```json
{
  "key": "f1",
  "command": "workbench.action.showCommands"
}
```



- [x] jump to specific line `Ctrl + g`
- [x] create new file

```json
{
  "key": "shift+alt+n",
  "command": "welcome.showNewFileEntries"
}
```

- [x] shell command: install 'code' command in PATH

- [x] trigger suggest

```json
{
  "key": "cmd+i", // ctrl + space2
  "command": "editor.action.triggerSuggest",
  "when": "editorHasCompletionItemProvider && textInputFocus && !editorReadonly && !suggestWidgetVisible"
}
```

- [x]  trigger parameter hint 

```json
{
  "key": "shift+cmd+space",
  "command": "editor.action.triggerParameterHints",
  "when": "editorHasSignatureHelpProvider && editorTextFocus"
}
```

- [x] close parameter hint

```json
{
  "key": "escape",
  "command": "closeParameterHints",
  "when": "editorFocus && parameterHintsVisible"
}
```



- [x] find and replace



```json
{
  "key": "alt+cmd+f",
  "command": "editor.action.startFindReplaceAction",
  "when": "editorFocus || editorIsOpen"
}
```

- [x] toggle panel visibility

```json
{
  "key": "cmd+j",
  "command": "workbench.action.togglePanel"
}
```

- [x] rename symbol

```json
{
  "key": "f2",
  "command": "editor.action.rename",
  "when": "editorHasRenameProvider && editorTextFocus && !editorReadonly"
}
```

- [x] shrink selection

```json
{
  "key": "ctrl+shift+left",
  "command": "editor.action.smartSelect.shrink",
  "when": "editorTextFocus"
}
```

- [x] expand selection

```json
{
  "key": "ctrl+shift+right",
  "command": "editor.action.smartSelect.expand",
  "when": "editorTextFocus"
}
```



- [x] show quick fix

```json
{
  "key": "cmd+.",
  "command": "editor.action.quickFix",
  "when": "editorHasCodeActionsProvider && textInputFocus && !editorReadonly"
}
```

## config user snippet





exhaustively check （穷尽检查）





- [x] font family

```json
  "editor.fontFamily": "Fira Code, jetBrains Mono",
  "terminal.integrated.fontFamily": "Fira Code, jetBrains Mono, monospace",
  "editor.fontLigatures": true,
  "editor.fontWeight": "500", // Retina
  "editor.fontSize": 16, 
```





![dev_tool](images/dev_tool_shortcut.png)
