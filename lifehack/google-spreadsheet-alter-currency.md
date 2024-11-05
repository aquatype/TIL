통화 드롭다운 값이 변경되었을 때 액수 셀의 넘버포맷을 변경하는 App Script.  

```javascript
const SHEET_NAME = 'NAME_OF_SHEET';
const CURRENCY_COLUMN = 'C';
const CURRENCY_SIGN = {
  KRW: '₩',
  JPY: '¥',
};

function onEdit(e) {
  let sheet = e.source.getActiveSheet();
  if (sheet.getName() !== SHEET_NAME || e.range.getColumn() !== CURRENCY_ROW.toUpperCase()) return;
  sheet.getRange(e.range.getRow(), e.range.getColumn()+1).setNumberFormat(`${CURRENCY_SIGN[e.value]}#,###`);
}
```
