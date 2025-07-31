# Pinia getter緩存優化技巧

```javascript
// 優化版本：緩存篩選結果
getters: {
  getActiveUserById(state) {
    // 這個篩選只在 state 改變時執行一次
    const activeUsers = state.users.filter((user) => user.active)
    // 使用已緩存的 activeUsers 進行查找
    return (userId) => activeUsers.find((user) => user.id === userId)
  }
}

// 未優化版本：每次調用都重新篩選
getters: {
  getUserById(state) {
    // 每次調用都會重新執行 filter
    return (userId) => state.users
      .filter(user => user.active)
      .find(user => user.id === userId)
  }
}

// 在模板中使用
<template>
  <!-- 直接調用函數，不需要 .value -->
  <p>User 2: {{ getUserById(2) }}</p>
</template>

// 在 setup 中使用
<script setup>
// 如果使用 storeToRefs，需要 .value
const { getUserById } = storeToRefs(userList)
const user = getUserById.value(2)
</script>
```

## 原理解釋

1. **函數返回機制:**
    - 當 getter 返回一個函數時，它實際上創建了一個"工廠函數"模式。
2. **緩存行為:**
    - 外層函數（`getActiveUserById`）只運行一次，創建 `activeUsers` 數組
    - 內層返回的函數每次調用都會運行，但它使用閉包中已緩存的 `activeUsers`
    - 這提供了一個優化：`filter` 操作只在狀態改變時執行一次，而不是每次查詢用戶時都執行

## 使用場景建議

- 需要根據參數查詢數據
- 需要在查詢前進行複雜數據處理
- 性能有較高要求的大型數據集處理
