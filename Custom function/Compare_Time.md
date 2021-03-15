Compare time value of two cells. Return Completed or Ongoing.
```js
/**
 * Compare time value of two cells. Return Completed or Ongoing.
 *
 * @param {date} start The cell with start time.
 * @param {date} end The cell with end time.
 * @return Completed|Ongoing base on First greater than Second or not.
 * @customfunction
 */
function compareTime(start, end) {
  var first = new Date(1, 1, 1, start.getHours(), start.getMinutes(), start.getSeconds());
  var second = new Date(1,1,1, end.getHours(), end.getMinutes(), end.getSeconds());
  var result
  if (first>second) {result = 'Completed'} else {result = 'Ongoing'}
  return result;
}
```
