Tạo data table web app để hiển thị dữ liệu từ Google Sheets sử dụng Apps Script.

## Code Apps Script

``` javascript
// Code.gs
var ss = SpreadsheetApp.getActiveSpreadsheet();
var sheet = ss.getSheetByName('Invoice');

function doGet(e) {
  var page = e.parameter.p || 'Index';
  return HtmlService.createTemplateFromFile(page)
    .evaluate();
}

function include(filename) {
  return HtmlService.createHtmlOutputFromFile(filename)
    .getContent();
}

function getAllData() {
  var range = sheet.getRange(2, 1, sheet.getLastRow()-1, sheet.getLastColumn());
  var values = range.getValues();
  return {
    header: getHeaderArr(),
    data: values
  }
}

function searchByPhone(input) {
  var range = sheet.getDataRange();
  var data = range.getValues();
  var header = data[0];
  var phoneCol = header.indexOf('Phone');
  var result = data.filter(function(row) {
      return row[phoneCol] === input
    });
  return {
    header: getHeaderArr(),
    data: result
    };
}

function getHeaderArr() {
  var headerRowPosition = 1; 
  var range = sheet.getRange(headerRowPosition, 1, 1, sheet.getLastColumn());
  return range.getValues()[0];
}

function test() {
  Logger.log(getHeaderArr());
}
```

## Page hiển thị data table

``` html
<!-- File Index.html -->
<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    <!-- This CSS package applies Google styling; it should always be included. -->
    <link rel="stylesheet" href="https://ssl.gstatic.com/docs/script/css/add-ons1.css">

    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.10.24/css/jquery.dataTables.css">
    <?!= include('Stylesheet') ?>
  </head>
  <body>
    <div class="heading">
      <h1>Invoice</h1>
      <p class="grey">Invoice data from the spreadsheet, use <a href="https://datatables.net/" target="_blank">Data Table jQuery</a> plug-in to render table.</p>
      <div id="loading">
      </div>
    </div>

    <div>
      <table id="table" class="display" width="75%"></table>
    </div>
    <script
      src="https://code.jquery.com/jquery-3.6.0.min.js"
      integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4="
      crossorigin="anonymous"></script>
  
    <script type="text/javascript" charset="utf8" src="https://cdn.datatables.net/1.10.24/js/jquery.dataTables.js"></script>

    <?!=  include('JavaScript') ?>
  </body>
</html>
```


## Page với chức năng search và hiển thị kết quả tìm kiếm

``` html
<!-- File Search.html -->
<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    <!-- This CSS package applies Google styling; it should always be included. -->
    <link rel="stylesheet" href="https://ssl.gstatic.com/docs/script/css/add-ons1.css">

    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.10.24/css/jquery.dataTables.css">
    <?!= include('Stylesheet') ?>
  </head>
  <body>
    <div class="heading">
      <h1>Search your invoice.</h1>
      <p>Input your phone number to search your invoice. Don't include country code, space and other special character.</p>
    </div>
    
    <div class="search-box">
      <input id="searchText" type="text">
      <button id="searchBtn" onclick="handleSearch()">Search</button>
    </div>

    <div class="response" id="status-box">
      <table id="result" class="display" width="75%"></table>
    </div>
    <script
      src="https://code.jquery.com/jquery-3.6.0.min.js"
      integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4="
      crossorigin="anonymous"></script>
  
    <script type="text/javascript" charset="utf8" src="https://cdn.datatables.net/1.10.24/js/jquery.dataTables.js"></script>

    <?!=  include('JavaScript') ?>
  </body>
</html>
```


## Code JavaScript client-side

``` javascript
// File JavaScript.html
<script>
  $().ready(function(){
    google.script.run.withSuccessHandler(showDataTable)
      .getAllData();
  });

  function showDataTable(dataTable) {
    $('#loading').hide();
    var header = [];
    dataTable.header.forEach(item => {
       header.push({title: item});
    })

    $('#table').DataTable({
      data: dataTable.data,
      columns: header,
      searching: false,
      info: false
    })
  }
</script>

<script>
  function handleSearch() {
    // remove data
    if ($.fn.DataTable.isDataTable('#result')) {
      $('#result').DataTable().clear().draw();
    };
    $('#searchBtn').attr('disabled', true);

    // get value
    let input = document.getElementById('searchText').value;
    
    // validation

    // request server side
    google.script.run
      .withSuccessHandler(onSuccess)
      .withFailureHandler(onFailure)
      .searchByPhone(input);

    // render response see function onSuccess()
  };

  function onSuccess(result) {
    //window.alert(result);
    if ($.fn.DataTable.isDataTable('#result')) {
      appendDataTable('#result', result.data);
    } else {
      renderTable('#result', result);
    }

    $('#searchBtn').removeAttr('disabled');
  };

  function appendDataTable(elementSelector, rowData) {
    $(elementSelector).DataTable().rows.add(rowData).draw();
  }

  function renderTable(elementSelector, option) {
    var table = $(elementSelector);
    var field = [];
    option.header.forEach(name => {
      field.push({title: name});
    });
    table.DataTable({
      data: option.data,
      columns: field,
      paging: false,
      ordering: true,
      info: false,
      searching: false
      }); 
  }

  function onFailure() {
    window.alert('Đã xảy ra lỗi khi truy cập file.')
  }
</script>
```

## Code CSS

``` html
<!-- File Stylesheet.html -->
<style>
  #loading {
    margin: auto;
    border: 16px solid #f3f3f3;
    border-radius: 50%;
    border-top: 16px solid #3498db;
    width: 120px;
    height: 120px;
    -webkit-animation: spin 2s linear infinite; /* Safari */
    animation: spin 2s linear infinite;
  }

  /* Safari */
  @-webkit-keyframes spin {
    0% { -webkit-transform: rotate(0deg); }
    100% { -webkit-transform: rotate(360deg); }
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
  .heading {
    text-align: center;
    margin: 12px;
  }
  .search-box {
    text-align: center;
    padding: 32px;
  }
</style>
```
