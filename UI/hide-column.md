# Hide-column
Hide one (or many) column of Google Spreadsheet by apps script. Input parameter by prompt, range or variable.

`Custom-menu.gs`
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
Hide columns based on a range in the active sheet. Input column name to a range before selecting function from custom menu.

```javascript
function hideColByRange() {
  let ss = SpreadsheetApp.getActiveSpreadsheet();
  let sheet = ss.getActiveSheet();
  let range = sheet.getActiveRange();
  let values = range.getValues();
  var chain = ''; // Combine array of values to a string
  
  values.forEach(row => {
    chain = chain.concat(row,' ');
  });
  
  let columnName =  chain.replace(',', ' ').trim().split(' ');
  let ui = SpreadsheetApp.getUi();
  let response = ui.alert('Do you want to hide column: ' + columnName);
  if (response == ui.Button.OK) {
      hideCol(columnName);
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
## Reference
* [Custom menu in G Suite](https://developers.google.com/apps-script/guides/menus)
* [Class UI](https://developers.google.com/apps-script/reference/base/ui)
