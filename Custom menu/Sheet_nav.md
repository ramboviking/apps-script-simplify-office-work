Navigator custom menu for Google Sheets using Apps Script.


![](https://images.viblo.asia/b0f269ec-e1fa-4ea3-b6ae-4c263dad31d1.png)

``` javascript
var ss = SpreadsheetApp.getActiveSpreadsheet();
var sheets = ss.getSheets();

function onOpen() {
  var ui = SpreadsheetApp.getUi();
  var nav = ui.createMenu('Navigate');

  nav.addItem('Show all', 'showAllSheet');
  nav.addSeparator();

  sheets.forEach(function(sheet) {
    nav.addItem(sheet.getName(), 'vk' + sheet.getSheetId());
  })

  nav.addToUi();
}

sheets.forEach(function(sheet) {
  var id = sheet.getSheetId();
  this['vk' + id] = function() {
    getSheetById(id).showSheet();

    var filteredSheets = sheets.filter(function(item) {return item.getSheetId() != id});
    filteredSheets.forEach(function(disableSheet) {
      getSheetById(disableSheet.getSheetId()).hideSheet();
    })
  }

  function getSheetById(id) {
  return SpreadsheetApp.getActive().getSheets().filter(
    function(s) {return s.getSheetId() === id;}
  )[0];
}
})

function showAllSheet() {
  sheets.forEach(function(sheet) {
    sheet.showSheet();
  })
}
```
