# Web常見用語解釋

>簡單解釋一些常見但看到會忘記的單字

[TOC]

## 常見用語

### 開發模式(Development Mode)

專門為程式開發階段設計的環境:

1. 系統會提供詳細的錯誤訊息，方便開發人員快速找出並修正問題
2. 程式碼不會被壓縮或最佳化，便於除錯和測試
3. 熱重載（Hot Reload）功能，讓修改立即顯示
4. 可能會包含測試數據和開發工具

### 生產模式(Production Mode)

專門為實際運營而設計的環境:

1. 程式碼會被最小化和最佳化
2. 移除除錯相關工具與訊息
3. 使用真實的資料庫連接和設定

### 依賴項（dependencies）

專案所需的第三方套件或模組

```javascript
//React 為一個依賴項
import React from 'react'
```

### 緩存(cache)

> 暫時的資料儲存區域，用來存最常使用的資料副本。

瀏覽器快取（Browser Cache）

> 當你第一次造訪一個網站時，瀏覽器會將資源下載並儲存在使用者電腦中。下次再訪問網站時，瀏覽器就會直接從本地讀取這些檔案，而不需要重新從網路下載，這樣可以大幅提升載入速度。
[使用control+F5可以完整重新加載網頁](https://www.freecodecamp.org/chinese/news/the-difference-between-f5-and-ctrl-f5/)

#### 伺服器快取（Server Cache）

> 網站伺服器也會將常用的資料儲存在記憶體中。

#### CDN 快取（Content Delivery Network Cache)

> 大型網站通常會使用分布在世界各地的 CDN 伺服器來儲存網站內容的副本。

### 裝飾器(Decorator)

> 裝飾器是一種特殊的語法，它允許我們以聲明式的方式為類、方法或屬性添加額外的功能或信息。在 TypeScript/NestJS 中，裝飾器使用 @ 符號。

```typescript
@Controller('users')  // 這是一個裝飾器
export class UsersController {
    @Get()  // 這也是一個裝飾器
    findAll() {
        return '這個方法會處理 GET /users 請求';
    }
}
```

### 工廠模式

>可以把它想像成使用者今天傳入某個數據，工廠函式根據數據產生不同回應，
>這樣跟一般的if/else的判斷是差在哪裡?可以看一下這段code

```typescript
// 一般的條件判斷
class BadExample {
    handleRequest(type: string) {
        if (type === 'A') {
            // 直接在這裡處理 A 類型的邏輯
            console.log('處理 A');
            // 可能還有很多相關程式碼
        } else if (type === 'B') {
            // 直接在這裡處理 B 類型的邏輯
            console.log('處理 B');
            // 可能還有很多相關程式碼
        }
    }
}

// 工廠模式的實作
interface Product {
    operation(): void;
}

class ProductA implements Product {
    operation() {
        console.log('處理 A');
    }
}

class ProductB implements Product {
    operation() {
        console.log('處理 B');
    }
}

@Injectable()
class ProductFactory {
    createProduct(type: string): Product {
        if (type === 'A') return new ProductA();
        if (type === 'B') return new ProductB();
        throw new Error('未知的產品類型');
    }
}
```

>可以了解到工廠的設計模式是更注重在使用者更需要甚麼，而不是如何產生出使用者需要的東西
>這會有以下幾點好處
>
>1. **封裝複雜性**
    >使用這個服務的人不需要知道如何創建日誌記錄器，他們只需要告訴工廠他們需要什麼類型的記錄器。
>2. **抽象與實作分離**
    >工廠通常會依賴於抽象（介面或抽象類別），而不是具體實作。這讓系統更容易擴展，因為新增新的實作不需要修改工廠的使用者。
>3. **關注點分離**
    >工廠的主要職責是「創建物件」，而不是處理業務邏輯。每個被創建的類別都負責自己的具體實作。這讓程式碼更容易維護和測試。。

### 路由前綴(prefix)

> 在一個網站中，我們會有不同的網址路徑（URL）來處理不同的功能。
> 前綴就是幫這些路徑加上一個共同的開頭，可以使相同路由的資源都歸納在同一個 Controller 裡面。
> 根據 RESTful API 的慣例，我們通常會把路由改成複數形式

```typescript
@Controller('todo')  // 設定前綴為 'todo'
export class TodoController {
    @Get()  // 處理 GET 請求
    findAll() {
        return '所有待辦事項';
    }

    @Get(':id')  // 處理帶有 ID 的 GET 請求
    findOne(@Param('id') id: string) {
        return `待辦事項 ${id}`;
    }
}
```

### 通用路由符號

>通用路由符號讓我們能用更靈活的方式來定義路由。想像這些符號就像是一種「萬用字元」，可以匹配多種不同的 URL 模式。

```typescript
@Controller('products')
export class ProductController {
    @Get('*')  // 星號是最基本的通用符號
    findAll() {
        return '這個方法會匹配任何 products/ 底下的路徑';
    }
}

@Controller('files')
export class FileController {
    @Get('**.jpg')  // 雙星號更強大
    getJpgFiles() {
        return '匹配所有 .jpg 結尾的檔案路徑';
    }

    @Get('docs/**/backup')  // 雙星號可以匹配多層路徑
    getBackups() {
        return '匹配所有 backup 目錄，不管它在哪一層';
    }
}
```

### 路由參數(Path Parameters)

>路由參數是在 URL 中傳遞數據的一種方式，用於標識特定資源，通常用冒號(:)來標記。

```javascript
//http://example.com/users/123

app.get('/users/:id', (req, res) => {
    const userId = req.params.id;
    // 使用 userId 獲取用戶信息
});
```

### 查詢參數(Query Parameters)

>查詢參數是在 URL 中傳遞數據的一種方式，出現在URL的問號(?)之後,多個參數之間用&符號連接。

```html
http://example.com/products?category=electronics&sort=price&order=desc
```

>查詢參數通常用於:
>
> - 篩選資料
> - 排序
> - 分頁
> - 搜尋

### 標頭(Headers)

>HTTP Headers 是在客戶端和伺服器之間傳遞的額外資訊，包含在 HTTP 請求和回應中。
>使用情境:
>
> - 身份驗證
> - 快取策略
> - CORS 政策

### 中介層(Middleware)

>是在路由處理請求之前執行的函數。
>
>1. 存取請求和回應物件 (request & response)
>2. 執行額外的邏輯
>3. 修改請求和回應
>4. 結束請求-回應循環
>5. 呼叫堆疊中的下一個 middleware

