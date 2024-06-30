---
draft: true
date: 2022-01-31
categories:
  - Hello
  - World
---

## mkdocs 配置

### 标题定制化

```css
.md-typeset h2 {
  font-size: 22px; display: inline-block; padding-left: 10px; border-left: 5px solid rgb(145, 109, 213); font-weight:bold;"
}
.md-typeset h3 {
  text-align: center;
  font-size: 20px;
  font-weight: bold;
  text-decoration: underline;
  text-decoration-color:rgb(145, 109, 213) ;
  text-underline-offset: 5px;
  text-decoration-thickness: 3px;
  /* position: relative; 使 ::after 定位相对于 h3 */
}
```

### 颜色微调

```css
[data-md-color-accent='purple'] {
  --md-accent-fg-color: rgb(145, 109, 213);
  --md-accent-fg-color--transparent: rgba(145, 109, 213, 0.2);
  --md-accent-bg-color: #fff;
  --md-accent-bg-color--light: #ffffffb3;
}
```

### [Editor/Window Management](https://code.visualstudio.com/docs/getstarted/keybindings#_editorwindow-management)

| Command                              | Key        | Command id                                   |
| :----------------------------------- | :--------- | :------------------------------------------- |
| New Window                           | ⇧⌘N        | `workbench.action.newWindow`                 |
| Close Window                         | ⌘W         | `workbench.action.closeWindow`               |
| Close Editor                         | ⌘W         | `workbench.action.closeActiveEditor`         |
| Close Folder                         | ⌘K F       | `workbench.action.closeFolder`               |
| Cycle Between Editor Groups          | unassigned | `workbench.action.navigateEditorGroups`      |
| Split Editor                         | ⌘\         | `workbench.action.splitEditor`               |
| Focus into First Editor Group        | ⌘1         | `workbench.action.focusFirstEditorGroup`     |
| Focus into Second Editor Group       | ⌘2         | `workbench.action.focusSecondEditorGroup`    |
| Focus into Editor Group on the Left  | unassigned | `workbench.action.focusPreviousGroup`        |
| Focus into Editor Group on the Right | unassigned | `workbench.action.focusNextGroup`            |
| Move Editor Left                     | ⌘K ⇧⌘←     | `workbench.action.moveEditorLeftInGroup`     |
| Move Editor Right                    | ⌘K ⇧⌘→     | `workbench.action.moveEditorRightInGroup`    |
| Move Editor into Next Group          | ⌃⌘→        | `workbench.action.moveEditorToNextGroup`     |
| Move Editor into Previous Group      | ⌃⌘←        | `workbench.action.moveEditorToPreviousGroup` |

|                | New lines | Spaces and tabs | Text wrapping |
| :------------- | :-------- | :-------------- | :------------ |
| `offsetHeihgt` | Collapse  | Collapse        | Wrap          |
| `clientHeight` | Preserve  | Preserve        | No wrap       |
| `scollHeight`  | Collapse  | Collapse        | No wrap       |
