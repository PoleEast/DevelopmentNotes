# 如何在前後端分離的專案中建立共享Library

`這篇筆記僅介紹本地套件的方式`

## 先在專案資料夾的根目錄建立Library的資料夾

```bash
mkdir test_library
cd test_library
npm init -y

# 安裝 TypeScript
npm install typescript -D
```

## 設定需要共享的函式或型別

```typescript
//src/types/auth.ts
export interface RegisterRequest {
  account: string
  nickname: string
  password: string
}

//src/index.ts
export * from "./types/auth";
```

## 配置package.json

```json
{
  "name": "test_library",   // 套件名稱
  "version": "1.0.0",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "tsc",
    "watch": "tsc -w"
  },
  "devDependencies": {
    "typescript": "^5.7.3"
  }
}

```

## 配置tsconfig.json

```jsonld
{
  "compilerOptions": {
    "target": "ES2017",
    "module": "CommonJS",
    "declaration": true,
    "strict": true,
    "outDir": "./dist"
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```

- declaration:這是最重要的設定，它會生成 `.d.ts` 型別定義檔案，讓其他專案可以引用這些型別。
- outDir: 指定編譯後的檔案輸出位置，確保原始碼和編譯後的檔案分開存放。
- module: 因為後端使用 NestJS（Node.js 環境），所以需要使用 CommonJS 模組格式。

## 在前後端專案安裝套件

```bash
npm install ../test_library
```

## 完成與維護

之後如果有更改相關型別要重新建置`test_library`與安裝套件
