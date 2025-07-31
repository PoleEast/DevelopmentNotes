# 防抖(debounce)使用與介紹

防抖 (debounce) 是指，將多次操作優化為，只在最後一次執行。具體來說，當一定時間內沒有持續觸發事件時，事件處理函式才會被執行一次，但如果在設定的時間內又一次觸發了事件，就會重新開始計時。

```javascript
function debounce(func, delay) {
    // 用來儲存計時器的 ID
    let timeoutId;
    
    // 返回一個新的函式
    return function (...args) {
        // 每次函式被調用時，先清除之前的計時器
        clearTimeout(timeoutId);
        
        // 設置一個新的計時器
        timeoutId = setTimeout(() => {
            // delay 毫秒後執行原始函式
            func.apply(this, args);
        }, delay);
    };
}

// 使用範例
const searchContacts = (searchText) => {
    console.log('搜尋聯絡人：', searchText);
    // 實際的搜尋邏輯...
};

// 創建一個防抖版本的搜尋函式
const debouncedSearch = debounce(searchContacts, 300);

// 當使用者輸入時
input.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
});

//時間軸：
//0ms: 輸入 "J" → 設置 300ms 計時器
//100ms: 輸入 "o" → 清除前一個計時器，設置新的 300ms 計時器
//150ms: 輸入 "h" → 清除前一個計時器，設置新的 300ms 計時器
//200ms: 輸入 "n" → 清除前一個計時器，設置新的 300ms 計時器
//: 使用者停止輸入，300ms 倒數完成 → 執行搜尋 "John"
```

## 應用場景

- **搜尋輸入框**：等使用者停止輸入後才發送請求
- **視窗調整**：等使用者調整完視窗大小後才重新計算佈局
- **表單驗證**：等使用者停止輸入後才進行驗證
- **按鈕點擊**：防止使用者短時間內重複點擊

## 防抖機制的優缺點

### 優點

- 減少不必要的函式調用
- 降低伺服器負載
- 提升應用程式效能
- 優化使用者體驗

### 缺點

- 延遲響應
- 複雜性增加
- 狀態管理
