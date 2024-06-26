

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
  "key": "cmd+i",
  "command": "editor.action.triggerSuggest",
  "when": "editorHasCompletionItemProvider && textInputFocus && !editorReadonly && !suggestWidgetVisible"
}
```

