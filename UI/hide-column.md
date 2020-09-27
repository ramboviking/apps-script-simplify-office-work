# Hide-column
Hide one (or many) column of Google Spreadsheet by apps script. Input parameter by prompt, range or object.

Custom-menu.gs
```javascript
function onOpen() {
  var ui = SpreadsheetApp.getUi();
  ui.createMenu('Your menu name')
    .addItem('Hide column prompt', 'HideColumnPrompt')
    .addSeparator()
    .addSubMenu(SpreadsheetApp.getUi().createMenu('View')
                .addItem('Hide column prompt', 'HideColumnPrompt')
                )
    .addToUi();
}
```
