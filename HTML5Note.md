# HTML+CSS 超濃縮語法筆記

`HTML5技術=HTML+CSS+JS APIs`

## HTML 基礎

- 現在 html 只會做基本的排版，風格或樣式會在 CSS 中鑽寫
- 超文本標記語言（英語：HyperText Markup Language，簡稱：HTML）是一種用於建立網頁的標準標記語言

### HTML 架構

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
  <meta charset="UTF-8" />

  <!-- 設置文檔的標題 -->
  <title>HTML 頁面標題</title>

  <!-- 設置視口 -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <!-- 提供文檔描述 -->
  <meta name="description" content="這是一個示範HTML <head>部分的頁面。" />

  <!-- 提供關鍵字 -->
  <meta name="keywords" content="HTML, 頁面, 教學" />

  <!-- 指定文檔作者 -->
  <meta name="author" content="作者姓名" />

  <!-- 刷新頁面 -->
  <meta http-equiv="refresh" content="30" />

  <!-- 加載外部CSS文件 -->
  <link rel="stylesheet" href="styles.css" />

  <!-- 引入外部JavaScript文件 -->
  <script src="script.js" defer></script>

  <!-- 定義網頁圖標 -->
  <link rel="icon" href="favicon.ico" type="image/x-icon" />
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

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/JjqYmrK)

### 文字段落

`內容、換行、分隔線`

```html
<p class="st1">this is p</p>
<span>this is span</span>
<div class="st1">
  <p>this is hr</p>
  <hr />
</div>
<div class="st1">
  <p>this is br</p>
  <br />
</div>
```

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/XWwmxPm)

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
  <hr />
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

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/MWdazzm)

### 表格

```html
<table>
  <caption>
    標題
  </caption>
  <!-- tr為列(row) th為行(Column)-->
  <!-- thead 為表頭、tbody為去頭去尾、tfoot為表尾 -->
  <thead>
    <tr>
      <th>表頭 1</th>
      <th>表頭 2</th>
      <th>表頭 3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>單元格 1</td>
      <td>單元格 2</td>
      <td>單元格 3</td>
    </tr>
    <tr>
      <td rowspan="2">合併兩行</td>
      <td>單元格 4</td>
      <td>單元格 5</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td>單元格</td>
      <td colspan="2">合併兩列</td>
    </tr>
  </tfoot>
</table>
```

[ColdePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/yLWObEG)

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

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/OJYyrLV)

### 圖片

`跟超連接概念差不多`

```html
<img src="URL" alt="替代文字" width="寬度" height="高度" />
<!--alt:查不到圖片時會顯示的文字  -->
```

### 表單

`用來收集用戶輸入數據的元素`

```html
<form action="submit_form.php" method="post">
  註冊表單
  <fieldset>
    <legend>帳號</legend>
    <label for="username">用戶名：</label>
    <input
      type="text"
      id="username"
      name="username"
      placeholder="輸入用戶名"
      required
    /><br /><br />

    <label for="password">密碼：</label>
    <input
      type="password"
      id="password"
      name="password"
      placeholder="輸入密碼"
      required
    /><br /><br />
  </fieldset>
  <fieldset>
    <legend>個人資料</legend>
    <label class="title">照片:</label>
    <input type="file" name="file" accept=".png,.jpg" />
    <br /><br />
    <div class="inline-labels">
      <label>性別：</label>
      <input type="radio" id="male" name="gender" value="male" />
      <label for="male">男</label>
      <input type="radio" id="female" name="gender" value="female" />
      <label for="female">女</label><br /><br />

      <label>愛好：</label>
      <input type="checkbox" id="hobby1" name="hobby" value="reading" />
      <label for="hobby1">閱讀</label>
      <input type="checkbox" id="hobby2" name="hobby" value="sports" />
      <label for="hobby2">運動</label><br /><br />
    </div>

    <label for="country">國家：</label>
    <select id="country" name="country">
      <option value="tw">台灣</option>
      <option value="us">美國</option>
      <option value="jp">日本</option></select
    ><br /><br />
  </fieldset>
  <fieldset>
    <legend>回饋</legend>
    <label for="message">訊息：</label>
    <textarea
      id="message"
      name="message"
      rows="4"
      cols="50"
      placeholder="輸入訊息"
    ></textarea
    ><br /><br />
  </fieldset>
  <input type="submit" value="提交" />
  <input type="reset" value="重置" />
</form>
```

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/WNBwOBN)

#### 常用 input type

```html
<!-- 文字輸入框 -->
<input
  type="text"
  id="username"
  name="username"
  placeholder="請輸入用戶名"
  required
/>

<!-- 密碼輸入框 -->
<label for="password">密碼：</label>
<input
  type="password"
  id="password"
  name="password"
  placeholder="請輸入密碼"
  required
/>

<!-- 電子郵件輸入框 -->
<label for="email">電子郵件：</label>
<input
  type="email"
  id="email"
  name="email"
  placeholder="請輸入電子郵件"
  required
/>

<!-- 電話號碼輸入框 -->
<label for="phone">電話號碼：</label>
<input
  type="tel"
  id="phone"
  name="phone"
  placeholder="請輸入電話號碼"
  required
/>

<!-- 網站網址輸入框 -->
<label for="website">網站網址：</label>
<input
  type="url"
  id="website"
  name="website"
  placeholder="請輸入網站網址"
  required
/>

<!-- 數量輸入框 -->
<label for="quantity">數量：</label>
<input type="number" id="quantity" name="quantity" min="1" max="10" value="1" />

<!-- 音量範圍輸入框 -->
<label for="volume">音量：</label>
<input type="range" id="volume" name="volume" min="0" max="100" value="50" />

<!-- 生日選擇框 -->
<label for="birthdate">生日：</label>
<input type="date" id="birthdate" name="birthdate" />

<!-- 會議時間選擇框 -->
<label for="meeting-time">會議時間：</label>
<input type="time" id="meeting-time" name="meeting-time" />

<!-- 約會時間選擇框 -->
<label for="appointment">約會時間：</label>
<input type="datetime-local" id="appointment" name="appointment" />

<!-- 提交按鈕 -->
<input type="submit" value="提交" />

<!-- 重置按鈕 -->
<input type="reset" value="重置" />
```

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/xxNVLay)

#### 常用驗證表單輸入

```html
<!-- 必填屬性 -->
<label for="username">用戶名：</label>
<input
  type="text"
  id="username"
  name="username"
  placeholder="請輸入用戶名"
  required
/>

<!-- 最小值屬性 -->
<label for="age">年齡：</label>
<input type="number" id="age" name="age" min="18" />

<!-- 最大值屬性 -->
<label for="quantity">數量：</label>
<input type="number" id="quantity" name="quantity" max="10" />

<!-- 正則表達式模式 -->
<label for="postalcode">郵政編碼：</label>
<input type="text" id="postalcode" name="postalcode" pattern="[0-9]{5}" />

<!-- 自動聚焦 -->
<label for="fullname">姓名：</label>
<input type="text" id="fullname" name="fullname" autofocus />

<!-- 禁用 -->
<label for="comment">備註：</label>
<textarea id="comment" name="comment" disabled></textarea>

<!-- 隱藏 -->
<input type="hidden" id="userid" name="userid" value="123" />
```

### 內置框架(iframe)

'可用來嵌入網頁、地圖、YT 等等'

```HTML
    <iframe src="map-url" name="map1",allowfullscreen>地圖</iframe>
    <!-- 不可指定路口網站 -->
```

### HTML5 結構語意元素

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
  <img src="example.jpg" alt="示例圖片" />
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

## CSS 基礎

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

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/vYwNbGg)

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
p {
  text-shadow: h-shadow | v-shadow | blur | color;
}
```

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/mdYevLp)

### 超連結樣式

```css
/* 預設樣式 */
a:link {
  text-decoration: none;
}
/* 連覽過的樣式 */
a:visited {
  color: red;
}
/* 滑鼠指到的樣式 */
a:hover {
  color: green;
}
/* 選取的樣式 */
a:active {
  color: yellow;
}
```

### 清單符號樣式

```css
.list {
  /* 清單前使用的符號 */
  list-style-type: 很多;
  /* 清單前用圖片 */
  list-style-image: url();
  /* 符號的位置 */
  list-style-position: inside|outside;
}
```

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/MWdazzm)

### 邊框(border)

`設計元素的邊框`

```css
p{
    /* border: 尺寸| 邊框風格 | 顏色 */
    border: 3px double red
    border-radius: 20px;
}
```

[CodePen 邊框範例](https://codepen.io/prwwnaem-the-sasster/pen/jOobJbN)

### 方塊模型(Box Model)

`描述了元素的內容、內邊距、邊框和外邊距的排列和尺寸`

```txt

Box Model 示意圖
+---------------------------------------+
|               Margin                  |
|  +-------------------------------+    |
|  |             Border            |    |
|  |  +------------------------+   |    |
|  |  |        Padding         |   |    |
|  |  |  +-----------------+   |   |    |
|  |  |  |     Content     |   |   |    |
|  |  |  +-----------------+   |   |    |
|  |  +------------------------+   |    |
|  +-------------------------------+    |
+---------------------------------------+

```

#### 模型樣式

```css
.box {
  width: 200px; /* 內容區域寬度 */
  height: 100px; /* 內容區域高度 */
  padding: 20px; /* 內邊距 */
  border: 5px solid black; /* 邊框 */
  margin: 15px; /* 外邊距 */
  background-color: lightblue; /* 背景顏色 */
  float: left; /*浮動樣式*/
  /* 
  總寬度 = margin-left + border-left-width + padding-left + width + padding-right + border-right-width + margin-right
  總高度 = margin-top + border-top-width + padding-top + height + padding-bottom + border-bottom-width + margin-bottom 
*/
}

.ClearBox {
  clear: left; /*做側靜止浮動元素*/
}
```

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/yLWObEG)

### 表格樣式

```css
.table1 {
  width: 100%;
  border-collapse: collapse;
}

tbody tr:nth-child(2n) {
  background-color: blanchedalmond;
}

tbody tr:nth-child(2n + 1) {
  background-color: rgb(248, 163, 36);
}

tfoot {
  background-color: paleturquoise;
}
```

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/yLWObEG)

### 字型樣式

`提供使用者電腦內建以外的字型`

```css
/* 伺服器內的字型 */
@font-face {
  src: url(fonts/fontfamily.ttf);
  font-family: fontfamily.ttfs;
}

/* 提供字型的網站 */
@import url(fontUrl);
```

### 多欄版面

```css
.multicolumn {
  column-width: 200px; /* 寬度 */
  column-count: 3; /* 欄數 */
  column-gap: 20px; /* 欄間距 */
  column-rule: 1px solid black; /* 欄分隔線 */
}
```

### 背景樣式

```css
.background-example {
  background-color: lightblue; /* 背景顏色 */
  background-image: url("https://example.com/image.jpg"); /* 背景圖片 */
  background-repeat: no-repeat; /* 背景圖片不重複 */
  background-position: center; /* 背景圖片居中顯示 */
  background-size: cover; /* 背景圖片覆蓋整個區域 */
  background-attachment: fixed; /* 背景圖片固定 */
  width: 100%;
  height: 500px;
}
```

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/NWVNaba)

### Display 樣式

`定義元素在網頁中的顯示方式`

```css
.block {
  display: block;
}
```

[CodePen 範例](https://codepen.io/prwwnaem-the-sasster/pen/PovNgoL)
