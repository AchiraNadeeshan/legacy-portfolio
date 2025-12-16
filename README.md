# 🌐 Legacy Portfolio

[![Live Preview](https://img.shields.io/badge/Live%20Preview-Click%20Here-blue?style=flat-square&logo=github)](https://achiranadeeshan.github.io/legacy-portfolio)

This repository contains the **previous version** of my personal portfolio website, originally created for a university project. It is preserved here as a public reference and archival copy, as the site will no longer be updated.

> ⚠️ **Note:** Some personal information and claims on this site are fictional, created purely for the context of the academic project.



### 📸 Screenshot

![Screenshot of Legacy Portfolio](./screenshot.png)



## 🧾 About This Version

This version was created:
- To fulfill project requirements with a clean UI and basic interactivity
- Without any external frameworks or libraries
- With a functional contact form that stores submissions to Google Sheets via Apps Script



## ⚙️ Technologies Used

- **HTML**
- **CSS**
- **JavaScript (Vanilla)**
- **GitHub Pages** (for static site hosting)
- **Google Sheets + Google Apps Script** (for the contact form)



## 📬 Contact Form Integration

The contact form uses a **Google Apps Script Web App** to submit responses directly into a Google Sheet.



### 🔗 Output Google Sheet
[View Submission Sheet](https://docs.google.com/spreadsheets/d/1d0KB9SoDvNAvHcq305uLKrWmFNuhFgvW-GWPw1_DNT4/edit?usp=sharing)



### 📄 Google Apps Script Code (Server-side)
Deploy your own AppsScript using the following code.
```javascript
var sheetName = 'Sheet1'
var scriptProp = PropertiesService.getScriptProperties()

function intialSetup () {
  var activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet()
  scriptProp.setProperty('key', activeSpreadsheet.getId())
}

function doPost (e) {
  var lock = LockService.getScriptLock()
  lock.tryLock(10000)

  try {
    var doc = SpreadsheetApp.openById(scriptProp.getProperty('key'))
    var sheet = doc.getSheetByName(sheetName)

    var headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0]
    var nextRow = sheet.getLastRow() + 1

    var newRow = headers.map(function(header) {
      return header === 'timestamp' ? new Date() : e.parameter[header]
    })

    sheet.getRange(nextRow, 1, 1, newRow.length).setValues([newRow])

    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'success', 'row': nextRow }))
      .setMimeType(ContentService.MimeType.JSON)
  }

  catch (e) {
    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'error', 'error': e }))
      .setMimeType(ContentService.MimeType.JSON)
  }

  finally {
    lock.releaseLock()
  }
}
```


### 🔗 Web App Endpoint (used in `index.html`)
Make sure to replace the placeholder with your own Google Apps Script deployment URL:

```html
<script>
  const scriptURL = 'https://script.google.com/macros/s/AKfycbyNF_3LioyzKUteDT0hb2Ai-jP9RGoKYqkDWzmkCQCRipPfRxinCWw644VNXrBoZD49/exec';
  // contact form logic continues...
</script>
```



## 🚀 What's Next?

Check out the new and improved portfolio:  
👉 [https://achiranadeeshan.vercel.app](https://achiranadeeshan.vercel.app)



## 📜 License

This project is publicly available as a learning reference.  
Feel free to explore or adapt it to your own way.


