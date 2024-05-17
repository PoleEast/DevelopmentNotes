# HTML+CSS 超濃縮語法筆記

`HTML5技術=HTML+CSS+JS APIs`

## HTML基礎

### HTML架構

`超文本標記語言（英語：HyperText Markup Language，簡稱：HTML）是一種用於建立網頁的標準標記語言`

```HTML
<!DOCTYPE html>
<!-- DOCTYPE宣告 -->
<html>
    <head>
        <!-- 網頁相關資訊 -->
    </head>
    <body>
        <!-- 網頁內容 -->
    </body>
</html>
```

### head

`網頁相關資訊`

```html
<head>
    <!-- 設置字符編碼 -->
    <meta charset="UTF-8">
    
    <!-- 設置文檔的標題 -->
    <title>HTML 頁面標題</title>
    
    <!-- 設置視口 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- 提供文檔描述 -->
    <meta name="description" content="這是一個示範HTML <head>部分的頁面。">
    
    <!-- 提供關鍵字 -->
    <meta name="keywords" content="HTML, 頁面, 教學">
    
    <!-- 指定文檔作者 -->
    <meta name="author" content="作者姓名">
    
    <!-- 刷新頁面 -->
    <meta http-equiv="refresh" content="30">
    
    <!-- 加載外部CSS文件 -->
    <link rel="stylesheet" href="styles.css">
    
    <!-- 引入外部JavaScript文件 -->
    <script src="script.js" defer></script>
    
    <!-- 定義網頁圖標 -->
    <link rel="icon" href="favicon.ico" type="image/x-icon">
</head>
```

### 標題元素

```htnl
<!-- 標題元素 -->
<h1>主標題</h1>
<h2>次級標題</h2>
<h3>三級標題</h3>
<h4>四級標題</h4>
<h5>五級標題</h5>
<h6>六級標題</h6>
```
