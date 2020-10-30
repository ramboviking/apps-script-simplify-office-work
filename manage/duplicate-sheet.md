# Duplicate sheet

## Duplicate a template sheet to multiple spreadsheet
Template: a template sheet in the active spreadsheet.

Id: id of destination spreadsheet store in a column of active spreadsheet.

``` js
function appendSheetToMultipleSpreadsheet() {
  const templateSheetName = 'TEMPLATE-SHEET-NAME';
  const idSheetName = 'ID-SHEET-NAME';
  const columnNumberContainId = 1;
  const newSheetName = 'CHANGE-NEW-NAME-HERE';
  
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const sheet = ss.getSheetByName(idSheetName);
  const rawList = sheet.getRange(1, columnNumberContainId, sheet.getLastRow()).getValues();
  const list = rawList.map(item =>{
    return item[0]
  });
  
  const templateSheet = ss.getSheetByName(templateSheetName);
  for (i=0; i<list.length; i++){
    try {
      const destination = SpreadsheetApp.openById(list[i]);
      const newSheet = templateSheet.copyTo(destination);
      newSheet.setName(newSheetName);
    }
    catch(e) {Logger.log(list[i] + ' not a valid id.')}
  }
  
  ss.toast('Append sheets done.')
}
```
