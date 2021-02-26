Remove empty colunms - rows after lastColumn() and lastRow() of a sheet by Apps Script.

``` js
// @return {Sheet} The "sheet" object for chain.
function trimSheet() {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheet = ss.getSheetByName('ResponseSheet');

  for (i=sheet.getLastColumn()+1; i<=sheet.getMaxColumns(); i++) {
    sheet.hideColumns(i);
  };

  for (j=sheet.getLastRow()+1; j<= sheet.getMaxRows(); j++) {
    sheet.hideRows(j);
  };

  return sheet;
}
```

## Reference
* [getLastRow()](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getLastRow())
* [getLastColumn()](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getLastColumn())
* [getMaxRows()](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getMaxRows())
* [getMaxColumns()](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getMaxColumns())
