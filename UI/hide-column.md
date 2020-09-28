# Hide-column
Hide one (or many) column of Google Spreadsheet by apps script. Input parameter by prompt, range or object.

Custom-menu.gs
```javascript
function onOpen(){
  ui = SpreadsheetApp.getUi();
  ui.createMenu('Custom')
    .addItem('Hide col by prompt', 'hideColByPrompt')
    .addItem('Hide col by range', 'hideColByRange')
    .addItem('Hide col by variable', 'hideColByVariable')
    .addToUi();
}
```

## Hide columns by prompt
Create a prompt dialog for user to input which column need to hide. Each column name separate by a space.

```javascript
function hideColByPrompt() {
  var ui = SpreadsheetApp.getUi();
  var response = ui.prompt('Input columns to hide, separate by space');
  if (response.getSelectedButton() == ui.Button.OK) {
    let input = response.getResponseText();
    let prettyInput = input.replace(/\s+/g, ' ').trim().split(' ');
    hideCol(prettyInput);
  }
}

function hideCol(arrOfLetter) {
  let ss = SpreadsheetApp.getActiveSpreadsheet();
  let sheet = ss.getActiveSheet();
  for (i=0; i<arrOfLetter.length; i++) {
    if (arrOfLetter[i]) {
      sheet.hideColumn(sheet.getRange(arrOfLetter[i] + ':' + arrOfLetter[i]))
    }
  }
}
```

## Hide columns by range

## Hide coumns by variable
Input columns name directy in the code. Variable type 's array.

```javascript
function hideColByVariable() {
  var arrayOfColumn = ['A', 'C'];
  hideCol(arrayOfColumn);
}

function hideCol(arrOfLetter) {
  let ss = SpreadsheetApp.getActiveSpreadsheet();
  let sheet = ss.getActiveSheet();
  for (i=0; i<arrOfLetter.length; i++) {
    if (arrOfLetter[i]) {
      sheet.hideColumn(sheet.getRange(arrOfLetter[i] + ':' + arrOfLetter[i]))
    }
  }
}
```
