# Gasの基本的なやつ

## スプレットシート

### 基本コード

```javascript
function myFunction() {
    var ss = SpreadsheetApp.getActiveSpreadsheet()
    //シート選択
    var sheet = ss.getSheetByName("シート1")
    //  範囲選択
    // var range = sheet.getRange("B1")
    var range =sheet.getRange(3,6)
    var value = range.setValue("マムシ")
    
    //シート削除
    ss.deleteSheet(sheet)
    
    //色の変更
    range.setBackground("カラーコード")
    
    Logger.log(value)
}
```

### ファイルの検索

```javascript
function serach() {
    var ss = SpreadsheetApp.openByUrl("ファイルのURLを記載")

    var ss = SpreadsheetApp.openById("ファイルIDを記載")
    //※https://docs.google.com/spreadsheets/d/<ここの部分がファイルのIDになる>/edit#gid=0
}
```

### スプレット超基本
```javascript
//シートの複製

function myFunction() {
  var ss =SpreadsheetApp.getActiveSpreadsheet()
  var sheet =ss.getSheetByName("シート1")
  for(var i=2; i<=10; i++){
    var newSheet =sheet.copyTo(ss)
    newSheet.setName("シート"+i)
  }
}

//foreachによるスプレットシートの名前の変更
function myFunction() {
  var ss =SpreadsheetApp.getActiveSpreadsheet()
  var sheets =ss.getSheets()
  sheets.forEach(function(sheets,index){
    var num =index+1
    sheets.setName("sheet"+num)
  })
}

```


### メール
#### 自動メール返信とそのslack通知まで
```javascript
function autoReplay(e) {
  var values =e.values

  var companyName =values[1]
  var name =values[2]
  var email =values[3]
  var title=`${name}様　お問い合わせありがとうございました。`
  var body =`
  お名前：：${name}様
  この度はお問い合わせくださりありがとうございました。

  会社名：${companyName}
  お名前：${name}
  メールアドレス：：${email}

  担当から3営業日以内にご返信いたします。
  どうぞよろしくお願いいたします。
  `
  GmailApp.sendEmail(email,title,body)
  notifySlack(`
  新い問い合わせが届きました。
  会社名：${companyName}
  お名前：：${name}
  メールアドレス：${email}
  `)
}

function notifySlack(msg){
  var postUrl="ここにslackのincomingWebhookを入れる"
  var userName ="bot"

  var payloadObj={
    userName:userName,
    text:msg
  }

// json化
  var payloadJson =JSON.stringify(payloadObj)
  var options ={
    method:"post",
    contentType:"application/json",
    payload:payloadJson
  }
  UrlFetchApp.fetch(postUrl,options)
}

```



