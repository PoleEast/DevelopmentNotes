# 如何實作Vue組件之間的溝通?

`Props Down, Events Up`

## 什麼是 Props Down, Events Up?

在 Vue 中，父子組件之間的通信遵循單向資料流的原則，這個原則可以簡單理解為「Props Down, Events Up」（屬性向下傳，事件向上傳）：

- Props Down：父組件通過屬性(props)向子組件傳遞資料
- Events Up：子組件通過事件(events)向父組件傳遞訊息

## 為什麼需要這樣的設計？
這種設計模式有助於：

1. 維護資料流向的清晰性
2. 讓組件之間的依賴關係更容易理解
3. 方便追蹤資料的變化來源
4. 降低組件之間的耦合度

## 實際範例

### 底層概念

這是一個簡單的輸入框範例，可以了解Props Down, Events Up的概念

```typescript
<!-- Child.vue -->

<template>
    <input :value="props.message" @input="messageChange">
</template>

<script setup lang="ts">
    // 1. 定義 props：接收來自父組件的資料
    const props = defineProps<{
        message?: string
    }>()

    // 2. 定義可以發送的事件
    const emit = defineEmits<{
        change:[newValue: string]
    }>()
    
    // 3. 處理輸入事件
    const messageChange = (event: Event)=>{
        // 發送 change 事件給父組件
        emit('change',event.target.value)
    }
</script>


<!-- parents.vue -->

<template>
    <h1>{{ title }}</h1>
    <!-- 4. 使用子組件：傳入 props 和監聽事件 -->
    <Child :message="message" @change="changeTitle" />
</template>

<script setup lang="ts">
    const message=ref('')
    const title=ref('')
    
    // 6. 處理子組件發送的事件
    const changeTitle = (newValue:string) =>{
        title.value = newValue
    }
</script>
```

## 資料流說明

### Props Down（屬性向下傳）：

- 父組件通過 :message="message" 將資料傳給子組件
- 子組件通過 defineProps 定義並接收這個資料
- props 是唯讀的，子組件不能直接修改它


### Events Up（事件向上傳）：

- 當使用者在輸入框輸入內容時，觸發 input 事件
- 子組件通過 emit('change', value) 向上發送事件
- 父組件通過 @change="changeTitle" 監聽並處理這個事件

### 使用v-model達成(Vue3.4)

v-model是一個語法糖，他實際上是將:value和@update結合再一起，將上面的範例改成v-model的方式:

```typescript
<!-- Child.vue -->

<template>
    <input v-model = "modelValue">
</template>

<script setup lang="ts">
    const modelValue = defineModel<string>()
</script>


<!-- parents.vue -->

<template>
    <h1>{{ title }}</h1>
    <Child v-model:modelValue="title" />
</template>

<script setup lang="ts">
    const title=ref('')
</script>
```

v-model 實際上是幫我們處理了：

- 向下傳遞 props
- 向上傳遞更新事件
- 自動更新父組件的值

這就是為什麼 v-model 是一個語法糖，因為它簡化了 Props Down & Events Up 的寫法。