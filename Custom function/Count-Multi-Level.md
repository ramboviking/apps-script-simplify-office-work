# Count-Multi-Level
`Google Spreadsheet`
`CountM` 's a custom function for Google Spreadsheet. This function 'll count a range with multi column by vertical direction. It check from the right to left and restart from 1 when detect value different from previous row.

```javascript
/**
 * Count a range multi-level. Restart from 1 when the value in the left column change.
 * Only count vertical direction.
 *
 * @param {Array<Array<string>>} input The value or range of cells
 *     to count
 * @return Array of counting
 * @customfunction
 */
function countM(range) {
  var defaultValue = [1];
  var result = defaultValue;
  if(!range) {result.pop()};
  
  if(Array.isArray(range)) {
    for (let i=1; i<range.length ; i++) {
      let last = range[i].length - 1;
      let reset = false;
      if (range[i][last] != range[i-1][last]) {
        result.push(result[i-1]+1);
      } else {result.push(result[i-1])}
      A
      // check value in the left column
      for (let j=last-1; j>=0;j--) {
        if (range[i][j] != range[i-1][j]) {
          reset = true;
        }
      }
      
      if (reset) { result[i]=1 }
    } // Close for loop
  }
  
  return result
}
```

## Example

| A      | B       | C    |
| -------| --------| -----|

| Region | Product | Part | Id |
| -------| --------| ---- | ---|
| N | Car | Light | `=countM(A2:C18)` |
| N | Car | Wheel | 2  |
| N | Car | Wheel | 2  |
| N | Truck | Light | 1   |
| N | Truck | Horn |  2  |
| N | Crane | Horn |  1  |
| N | Crane | Belt |   2 |
| N | Crane | Fuel | 3  |
| S | Crane | Fuel |  1 |
| S | Car | Wheel |  1  |
| S | Car | Light | 2   |
| S | Crane | Horn | 1  |
| S | Crane | Light | 2  |
| S | Truck | Horn |  1 |
| N | Crane | Horn |  1 |

## Reference
* [Custom Functions in Google Sheets](https://developers.google.com/apps-script/guides/sheets/functions)
