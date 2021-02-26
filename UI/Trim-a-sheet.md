Remove empty colunms - rows after lastColumn() and lastRow() of a sheet by Apps Script.

``` js
// @return {Sheet} The "sheet" object for chain.
function trimSheet() {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheet = ss.getSheetByName('ResponseSheet');
  var lastRow = sheet.getLastRow();
  var lastColumn = sheet.getLastColumn();

  sheet.hideRows(lastRow +1, sheet.getMaxRows() - lastRow)
  sheet.hideColumns(lastColumn + 1, sheet.getMaxColumns() - lastColumn)

  return sheet;
}
```

## Reference
* [getLastRow()](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getLastRow())
* [getLastColumn()](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getLastColumn())
* [getMaxRows()](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getMaxRows())
* [getMaxColumns()](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getMaxColumns())
