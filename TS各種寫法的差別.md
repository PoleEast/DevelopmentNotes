# TS各種寫法的差別

## 箭頭函數VS傳統函數

```typescript
// 箭頭函數
const login = () => {
  // this 會繼承自外層作用域
}

// 傳統函數宣告
function login() {
  // this 會依據呼叫方式決定
}
```

### 主要差異

1. `this` 的綁定方式
    - 箭頭函數(=>)會自動繼承外層作用域的 `this`
    - 傳統函數宣告的 `this` 會依據怎麼被呼叫而改變
2.提升(Hoisting)行為
    - `function login(){}` 會被提升到作用域的最上方，所以可以在宣告前使用
    - `const login = () => {}` 不會被提升，必須在宣告後才能使用

## Interface(介面) VS type(類型別名) VS class(類別)

### Interface(介面)

`就像是一個"規格書"`

- 主要用於定義物件的結構和形狀
- 可以被擴展（extends）和實現（implements）
- 適合用於定義物件的契約

```typescript
// 定義 API 回應的資料結構
interface ApiResponse {
  success: boolean;
  message: string;
  data: any;
}

// 定義元件的 props
interface DialogProps {
  isOpen: boolean;
  title: string;
  onClose: () => void;
}
```

### type(類型別名)

`像是一個"別名"或"組合"`

- 可以為任何類型定義一個名字，包括基本類型、聯合類型、交叉類型等
- 更靈活，可以使用更多類型運算
- 不能被擴展，但可以使用交叉類型(&)來組合

```typescript
// 定義可能的值
type Currency = "TWD" | "USD" | "JPY";

// 組合多個類型
type UserResponse = SuccessResponse | ErrorResponse;
```

### class(類別)

`是一個包含資料和功能的"模板"`

- 是一個實際的藍圖，可以創建實例
- 包含實作細節和方法
- 可以有建構函式和私有成員
- 支援物件導向程式設計的特性

```typescript
// 建立一個可重複使用的功能
class TravelPlanner {
  private destinations: string[] = [];
  
  addDestination(place: string) {
    this.destinations.push(place);
  }
  
  generateItinerary() {
    // 生成行程表的邏輯
  }
}
```