---
author: "Marios Karamanis"
title: "Basic onEdit(e) application - utilize the event object"
date: "2020-11-26"
description: "Guide to a very simple onEdit trigger function"
tags: ["google-apps-script", "onEdit", "trigger"]
ShowToc: true
---

This article demonstrates the basic use of an ```onEdit(e)```
along with the use of the event object. The latter contains useful information
regarding the on edit interaction with the sheet.


### Simple ```onEdit(e)``` function

```javascript
function onEdit(e) {
  const as = e.source.getActiveSheet(); // get active sheet
  const row = e.range.getRow(); // get edited row
  const col = e.range.getColumn(); // get edited column
  /***
  check if:
  1) the name of the active sheet matches the desired sheet name.
  2) the edited column is A.
  3) the edited row is the 2nd or higher.
  if true, set the date to column B
  ***/
  if (as.getName() == "Sheet1" && col == 1 && row > 1){
  as.getRange(row,2).setValue(new Date());
  }
}
```


See the [official documentation](https://developers.google.com/apps-script/guides/triggers#onedite) for more details.
