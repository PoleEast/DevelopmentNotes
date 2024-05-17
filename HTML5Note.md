# HTML+CSS 超濃縮語法筆記

`HTML5技術=HTML+CSS+JS APIs`

## HTML基礎

- 現在html只會做基本的排版，風格或樣式會在CSS中鑽寫
- 超文本標記語言（英語：HyperText Markup Language，簡稱：HTML）是一種用於建立網頁的標準標記語言

### HTML架構

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

```html
<!-- 標題元素 -->
<!-- 每一個HTML只能有一個h1 -->
<h1>主標題</h1>
<h2>次級標題</h2>
<h3>三級標題</h3>
<h4>四級標題</h4>
<h5>五級標題</h5>
<h6>六級標題</h6>
```

[CodePen範例](https://codepen.io/prwwnaem-the-sasster/pen/JjqYmrK)

### 文字段落

`內容、換行、分隔線`

```html
    <p class="st1">this is p</p>
    <span>this is span</span>
    <div class="st1">
        <p> this is hr</p>
        <hr>
    </div>
    <div class="st1">
        <p> this is br </p>
        <br>
    </div>
```

[CodePen範例](https://codepen.io/prwwnaem-the-sasster/pen/XWwmxPm)

### 清單元素

`分為有序(ul)和無序(lo)`

```html
<body>
    <h2>清單li</h2>
    <ul>
        <li>項目1</li>
        <li>項目2</li>
        <li>項目3</li>
        <li>項目4</li>
    </ul>
    <hr>
    <h2>清單lo</h2>
    <ol>
        <li>項目1</li>
            <ol>
                <li>項目1-1</li>
                <li>項目1-2</li>
            </ol>
        <li>項目2</li>
    </ol>
</body>

```

[CodePen範例](https://codepen.io/prwwnaem-the-sasster/pen/MWdazzm)

### 超連結

`外部連結要有網頁路徑、同一個伺服器內部請使用相對路徑`

```html
<body>
    <a href="https://www.google.com.tw/?hl=zh_TW">google首頁</a>
    <a href="mailto:mail@gmail.com.tw">信箱</a>
    <a href="#book">連接到網頁特定位置(書籤)</a>
    <div id="book1">點擊書籤會到達的位置</div>
</body>
```

[CodePen範例](https://codepen.io/prwwnaem-the-sasster/pen/OJYyrLV)

### 圖片

`跟超連接概念差不多`

``` html
    <img src="URL" alt="替代文字" width="寬度" height="高度">
    <!--alt:查不到圖片時會顯示的文字  -->
```

### HTML5結構語意元素

`此為標準版型，沒有預設樣式請用CSS做調整`

#### main

`在典型的網頁布局中，<main>通常包含頁面的主要內容，例如文章內容、產品列表、用戶資訊等`

```HTML
<main>
    <h1>主要內容</h1>
    <p>這裡放置了網頁的主要內容，例如文章、產品列表等。</p>
</main>
```

#### header

`在頁面頂部通常會放置一個<header>元素，用於顯示網站的標題和導航欄`

```html
<header>
    <h1>網站標題</h1>
    <nav>
        <ul>
            <li><a href="#book1">首頁</a></li>
            <li><a href="#book2">關於我們</a></li>
            <li><a href="#book3">產品</a></li>
            <li><a href="#book4">聯繫我們</a></li>
        </ul>
    </nav>
</header>
```

#### footer

`在頁面底部通常會放置一個<footer>元素，用於顯示網站的版權信息和其他相關內容。`

```html
<footer>
    <p>&copy; 2024 - 網站版權所有</p>
    <nav>
        <ul>
            <li><a href="#">隱私政策</a></li>
            <li><a href="#">使用條款</a></li>
        </ul>
    </nav>
</footer>
```

#### article

`當需要標識一個獨立的、可獨立運作的內容塊時，可以使用<article>元素`

```html
<article>
    <h2>文章標題</h2>
    <p>這是一篇關於文章內容的描述。</p>
    <p>這是文章的正文內容。</p>
    <footer>
        <p>作者：某某某 | 發表日期：2024年5月17日</p>
    </footer>
</article>
```

#### nav

`當需要標識一個導航區域時，可以使用<nav>元素`

```html
<nav>
    <ul>
        <li><a href="#book1">首頁</a></li>
        <li><a href="#book2">關於我們</a></li>
        <li><a href="#book3">產品</a></li>
        <li><a href="#book4">聯繫我們</a></li>
    </ul>
</nav>
```

#### aside

`通常在<article>或<main>元素內部作為附加內容的側邊欄時使用。`

```html
<aside>
    <h2>側邊欄</h2>
    <p>這裡放置了與主要內容相關的附加信息，例如廣告、相關連結等。</p>
</aside>
```

#### figure

`當需要標識一個獨立的媒體內容時，可以使用<figure>元素。`

```html
<figure>
    <img src="example.jpg" alt="示例圖片">
    <figcaption>圖片說明：這是一個示例圖片。</figcaption>
</figure>
```

#### section

`當需要標識文檔中的一個具有語義的區域時，可以使用<section>元素。`

```html
<figure>
<section>
    <h2>區域標題</h2>
    <p>這是一個區域的內容，可以有自己的標題。</p>
</section>

</figure>
```

## CSS基礎

`階層式樣式表（英語：Cascading Style Sheets，縮寫：CSS，是一種用來為結構化文件添加樣式的電腦語言，由W3C定義和維護。`

### 選擇器(Selectors)

`找到要修改風格的段落、元素，CSS的核心`

#### 基本架構

```CSS
Selector{
    屬性:屬性值;
    屬性:屬性值
}
/* 請用;將不同屬性分開 */
```

#### 基本選擇器

```CSS
/* 元素選擇器：通過元素的名稱來選擇元素
。例如，p選擇所有段落元素。 */
p {
    color: blue;
}
/* 類選擇器：通過元素的class來選擇元素。
以 . 開頭，後接類名 */
.highlight {
    background-color: yellow;
}
/* ID選擇器：通過元素的ID來選擇元素。
以#開頭，後接ID名。 */
#header {
    font-size: 24px;
}
```

#### 進階選擇器

##### 屬性選擇器

```CSS
/* 屬性存在選擇器：選擇具有特定屬性的元素。*/
input[type] {
    border: 1px solid black;
}
/* 屬性值選擇器：選擇具有特定屬性值的元素。*/
input[type="text"] {
    background-color: lightgray;
}
```

##### 關係選擇器

```CSS
/* 後代選擇器：選擇元素的後代元素。使用空格來表示。*/
div p {
    margin: 0;
}
/* 子元素選擇器：選擇元素的直接子元素。使用>來表示。只拿一層*/
ul > li {
    list-style-type: none;
}
/* 相鄰兄弟選擇器：選擇某個元素後面相鄰的兄弟元素。使用+來表示。抓取同一層 */
h2 + p {
    font-style: italic;
}
/* 通用兄弟選擇器：選擇某個元素後面的所有兄弟元素。使用~來表示。抓同一層與所有後代 */
h2 ~ p {
    color: green;
}
```

##### 組合選擇器

```CSS
/* 多元素選擇器：選擇多個元素。使用逗號分隔。*/
h1, h2, h3 {
    font-family: Arial, sans-serif;
}
/* 群組選擇器：同時將多個選擇器應用到一個樣式。*/
.highlight, .important {
    font-weight: bold;
}
```

### 尺寸單位

```CSS
/* 相對單位 */
    /* em：相對於元素的字體大小。*/
    p {
        font-size: 1em;
    }
    /* rem：相對於根元素的字體大小。 */
    p {
        font-size: 1rem; 
    }
/* 絕對單位 */
    /* 像素（px）：指定一個固定的尺寸，不會隨著環境改變而改變。 */
    p {
        font-size: 16px;
    }
/* 百分比 */
    /* 使用百分比可以相對於父元素的尺寸進行調整。 */
    div {
        font-size: 120%; /* 相對於父元素的尺寸增加20% */
    }
/* 關鍵字 */
    /* 使用預定義的關鍵字，如small、medium、large等。 */
    p {
        font-size: large;
    }
```

[CodePen範例](https://codepen.io/prwwnaem-the-sasster/pen/vYwNbGg)

### 顏色

```CSS
/* 使用顏色名稱CSS支援140種顏色名稱。 */
p {
    color: red; 
    background-color: lightblue;
}
/* 使用十六進制顏色碼，以#開頭後跟三個或六個十六進制數字。 */
h1 {
    color: #ff0000;
    background-color: #add8e6;
}
/* 使用RGB顏色，每個值在0到255之間。 */
h2 {
    color: rgb(255, 0, 0); 
    background-color: rgb(173, 216, 230);
}
/*使用RGBA顏色，alpha值在0到1之間，表示透明度。*/
h3 {
    color: rgba(255, 0, 0, 0.5); 
    background-color: rgba(173, 216, 230, 0.5); 
}
```

### 文字

```css
p {
    /* 文字修飾:不增加|底線|上底線|刪除線  */
    text-decotation: none | underline | overline | line-throght
    /* 大小寫轉換:不改變|首字母大寫|大寫|小寫 */
    text-transform: none | capitalize | uppercase| lowercase
    /* 文字行距: */
    text-height: 尺寸 | 倍數
    /* 文字對齊 */
        /* 水平對齊 */
        text-align: center | right | left | justify
        /* 垂直對齊 !對大部分block沒有用，儲存格常用*/
        vertical-align: bottom | middle | top
    /* 文字字型 */
    font-family:字型名稱
    /* 文字粗細:100~900|400|700|比父大|比父小 */
    font-weight: 100~900| normal | bold | bolder |lighter
    /* 文字類型:正常|斜體|機械式斜體 */
    font-style: normal | italic | oblique
}
```

### 陰影

`分為text-shadow和box-shadow`

```css
/* 文字陰影:左右偏移|上下偏移|模糊程度|陰影顏色 */
p{
    text-shadow: h-shadow | v-shadow | blur | color 
}
```

[CodePen範例](https://codepen.io/prwwnaem-the-sasster/pen/mdYevLp)

### 超連結樣式

```css
/* 預設樣式 */
 a:link{
    text-decoration:none
 }
 /* 連覽過的樣式 */
 a:visited{
    color:red
 }
 /* 滑鼠指到的樣式 */
 a:hover{
    color:green
 }
 /* 選取的樣式 */
 a:active{
    color:yellow
 }
```

### 清單符號樣式

```css
    .list{
        /* 清單前使用的符號 */
        list-style-type:很多;
        /* 清單前用圖片 */
        list-style-image:url();
        /* 符號的位置 */
        list-style-position:inside|outside
    }
```

[CodePen範例](https://codepen.io/prwwnaem-the-sasster/pen/MWdazzm)

### 邊框(border)

`設計元素的邊框`

```css
p{
    /* border: 尺寸| 邊框風格 | 顏色 */
    border: 3px double red
}
```

[CodePen邊框範例](https://codepen.io/prwwnaem-the-sasster/pen/jOobJbN)