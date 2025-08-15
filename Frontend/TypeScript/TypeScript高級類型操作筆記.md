# TypeScript 高級類型操作筆記

## 映射類型 (Mapped Types)

### 基本語法
```typescript
{ [K in keyof T]: ... }
```

### 核心概念
- `in` 關鍵字用來遍歷類型的所有屬性
- `K` 是遍歷過程中的屬性名變數
- 類似 JavaScript 中的 `for...in` 迴圈，但作用在類型層面

### 實例演示
```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

// 基本映射
type UserOptional = { [K in keyof User]: User[K] | undefined };
// 結果：{ id: number | undefined; name: string | undefined; email: string | undefined; }
```

## 索引存取類型 (Indexed Access Types)

### 基本語法
```typescript
T[K]  // 獲取類型 T 中屬性 K 的類型
```

### 用法說明
```typescript
interface Person {
  id: number;
  name: string;
  active: boolean;
}

type PersonId = Person['id'];       // number
type PersonName = Person['name'];   // string
type PersonActive = Person['active']; // boolean
```

## 條件類型 (Conditional Types)

### 基本語法
```typescript
T extends U ? X : Y
```

### 兩種 extends 用法

#### 1. 類型擴展
```typescript
interface Animal { name: string; }
interface Dog extends Animal { breed: string; }  // 擴展類型
```

#### 2. 條件判斷
```typescript
string extends number ? 'yes' : 'no'  // 'no'
number extends number ? 'yes' : 'no'  // 'yes'
```

## 複合高級類型：提取數字屬性

### 完整語法分解
```typescript
{ [K in keyof T]: T[K] extends number ? K : never }[keyof T]
```

### 逐步解析

#### 步驟 1: 映射類型遍歷
```typescript
[K in keyof T]  // 遍歷 T 的所有屬性名
```

#### 步驟 2: 條件類型判斷
```typescript
T[K] extends number ? K : never
// 如果屬性值是 number 類型，返回屬性名 K
// 否則返回 never
```

#### 步驟 3: 索引存取提取
```typescript
[keyof T]  // 提取映射類型中所有值的聯合類型
```

### 完整實例
```typescript
interface Person {
  name: string;     // string extends number ? 'name' : never → never
  age: number;      // number extends number ? 'age' : never → 'age'
  height: number;   // number extends number ? 'height' : never → 'height'
  active: boolean;  // boolean extends number ? 'active' : never → never
}

// 映射結果
{
  name: never;
  age: 'age';
  height: 'height';
  active: never;
}

// 索引存取結果
never | 'age' | 'height' | never
// never 在聯合類型中被忽略
'age' | 'height'
```

## 實際應用：類型安全的數值計算

### 工具類型定義
```typescript
type NumberKeys<T> = {
  [K in keyof T]: T[K] extends number ? K : never;
}[keyof T];
```

### 函數實現
```typescript
const calculateAverage = <T>(
  array: T[], 
  key: NumberKeys<T>
): number => {
  return array.reduce((acc, cur) => {
    return acc + (cur[key] as number);
  }, 0) / array.length;
};
```

### 使用效果
```typescript
interface Data {
  name: string;
  score: number;
  rating: number;
}

const data: Data[] = [/* ... */];

calculateAverage(data, 'score');   // ✅ 正確
calculateAverage(data, 'rating');  // ✅ 正確  
calculateAverage(data, 'name');    // ❌ 類型錯誤
```

## 關鍵概念總結

1. **映射類型**：用 `[K in keyof T]` 遍歷類型屬性
2. **索引存取**：用 `T[K]` 獲取屬性的類型
3. **條件類型**：用 `extends` 進行類型判斷
4. **never 類型**：在聯合類型中會被自動忽略
5. **類型安全**：編譯期間就能防止類型錯誤

## 進階技巧

### 常用工具類型模式
```typescript
// 可選化所有屬性
type Partial<T> = { [K in keyof T]?: T[K] };

// 必需化所有屬性  
type Required<T> = { [K in keyof T]-?: T[K] };

// 只讀化所有屬性
type Readonly<T> = { readonly [K in keyof T]: T[K] };

// 提取特定類型的屬性名
type KeysOfType<T, U> = {
  [K in keyof T]: T[K] extends U ? K : never;
}[keyof T];
```

---
*筆記日期：2025-08-15*
*相關專案：PinPin 旅遊規劃平台*