

- [x] VScode光标在终端和编辑器快速移动

`View: Focus Last Editor Group`

  {
  "key": "shift+alt+0",
  "command": "terminal.focus"
  }

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

- [x]  trigger parameter hint (esc to close)

```json
{
  "key": "shift+cmd+space",
  "command": "editor.action.triggerParameterHints",
  "when": "editorHasSignatureHelpProvider && editorTextFocus"
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







![dev_tool](images/dev_tool_shortcut.png)
