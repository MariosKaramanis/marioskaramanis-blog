---
author: "Marios Karamanis"
title: "Generate timestamps upon edits - Basic onEdit(e) example"
date: "2020-11-26"
description: "Guide to a very simple onEdit trigger function"
tags: ["google-apps-script", "onEdit", "trigger"]
ShowToc: true
---

## Goal:

The goal of this script is to generate a *timestamp* on a particular *column* and *sheet* upon edits on cells of another column but same sheet. It is a very basic script and can be generalized to many other scenarios as well.

## The onEdit(e) trigger function:

This article demonstrates the basic use of an ```onEdit(e)```
along with the use of the event object. The latter contains useful information
regarding the on edit interaction with the sheet. The ```onEdit(e)``` trigger function is one of the most basic [simple triggers](https://developers.google.com/apps-script/guides/triggers#onedite). It runs automatically when a user changes the value of any cell in a spreadsheet. 

### The event object:
Many trigger functions, inlcuding ```onEdit(e)``` accepts an argument which is the [event object](https://developers.google.com/apps-script/guides/triggers/events). The name of this argument is up to you but the most common names are `e` or `event`. The `event object` contains information about the context that caused the trigger to fire. In this specific project we are interested in the following two objects:

1. `e.source` is equivalent to `SpreadsheetApp.getActive()` which returns an instance of the [Spreadsheet](https://developers.google.com/apps-script/reference/spreadsheet/spreadsheet) class. Therefore, we can apply the [getActiveSheet()](https://developers.google.com/apps-script/reference/spreadsheet/spreadsheet#getActiveSheet()) method to it in order to get the active sheet where `onEdit(e)` was fired. That is a very important concept because most of the times we just want to capture edits on particular sheets and not on every sheet. The idea here is to get the name of the active sheet and execute some code *only* when a particular sheet is edited.

2. `e.range` returns an instance of the [Range](https://developers.google.com/apps-script/reference/spreadsheet/range) class. This class unlocks a big variety of useful methods such as [getRow()](https://developers.google.com/apps-script/reference/spreadsheet/range#getrow) or [getColumn()](https://developers.google.com/apps-script/reference/spreadsheet/range#getcolumn) which allow us to know the row and column number of the cell that was edited. In this way we can control the range of cells for which we set the timestamp upon edits.

If we summarize the above into code, we define the active sheet, the row and the column of the edited cell:

```javascript
  const as = e.source.getActiveSheet(); // get active sheet
  const row = e.range.getRow(); // get edited row
  const col = e.range.getColumn(); // get edited column
```

The next step is to use an `if` statement to incorporate some conditions that will allow us to execute a block of code, in this case the timestamp creation and the `set` of its value, on particular edits. In this example, we want to react on changes:

1. in the sheet with the name `"Sheet1"`,
2. column `A` or column number `1`,
3. from row `2` *onwards* assuming we have a header in the 1st row.

If these conditions are satisfied we want to create a timestamp in column B. To get the cell in the same row as the edited cell but in column B or column number `2` we need to apply [getRange(row,2)](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getrangerow,-column) on the active sheet `as`. Remember `row` contains the row number of the edited cell. Finally, to set the value, we need to apply [setValue()](https://developers.google.com/apps-script/reference/spreadsheet/range#setvaluevalue) to the range object obtained before. To get the timestamp we simply take advantage of the [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) class and we create a new instance of this class.

The above can be translated in the following code block:

```javascript
  if (as.getName() == "Sheet1" && col == 1 && row > 1){
  as.getRange(row,2).setValue(new Date());
  }
```


## Code snippet:

Here is the complete code snippet of the solution:

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

## Installation:

Since this is a simple trigger, the installation process is straightforward and requires only 2 steps:

1. Click on **Tools => Script editor** on the top menu of your spreadsheet file.
2. Copy paste the aforementioned code snippet on a blank script in the script editor and save the changes.
