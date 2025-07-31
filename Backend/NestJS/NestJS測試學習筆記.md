# NestJS 測試學習筆記

## 基礎環境設定

### Jest + ESM + TypeScript 配置 (package.json)

```json
{
  "jest": {
    "preset": "ts-jest/presets/default-esm",        // 啟用 ESM 支援
    "extensionsToTreatAsEsm": [".ts"],              // 將 .ts 檔案視為 ESM 模組
    "globals": {
      "ts-jest": {
        "useESM": true,                             // 啟用 ESM 編譯
        "tsconfig": "tsconfig.test.json"            // 指定測試用的 TypeScript 配置
      }
    },
    "moduleNameMapper": {
      "^(\\.{1,2}/.*)\\.js$": "$1"                  // 處理 .js 匯入映射到 .ts 檔案
    },
    "transformIgnorePatterns": [
      "node_modules/(?!(.*\\.mjs$))"                // 排除 node_modules，但包含 .mjs 檔案
    ]
  }
}
```

### 測試檔案基本結構

```typescript
// 1. Mock 外部模組（檔案最上方）
jest.mock('bcrypt', () => ({
  compareSync: jest.fn(),
  hashSync: jest.fn(),
}));

// 2. Import 語句和 Mock 物件定義
import { Test, TestingModule } from '@nestjs/testing';
import { UserService } from './user.service.js';

const mockDependency = {
  method1: jest.fn(),
  method2: jest.fn(),
};

// 3. 測試套件結構
describe('ServiceName', () => {
  let service: ServiceName;
  
  beforeEach(async () => {
    jest.clearAllMocks();
    
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        ServiceName,
        { provide: Dependency, useValue: mockDependency },
      ],
    }).compile();
    
    service = module.get<ServiceName>(ServiceName);
  });
});
```

## 核心概念

### 1. Mocking 策略

#### Mock 方法類型

- **同步方法**：`mockReturnValue(value)`
- **異步成功**：`mockResolvedValue(value)`  
- **異步失敗**：`mockRejectedValue(error)`

#### 依賴處理

- **依賴注入服務**：`{ provide: Service, useValue: mockObject }`
- **外部模組**：`jest.mock('module-name')` 在檔案最上方

### 2. 測試結構（AAA 模式）

```typescript
it('should do something when condition is met', async () => {
  // Arrange - 準備測試資料和 Mock 行為
  const inputData = { /* 測試資料 */ };
  mockService.method.mockResolvedValue(expectedResult);
  
  // Act - 執行被測試的方法
  const result = await service.methodUnderTest(inputData);
  
  // Assert - 驗證結果和呼叫
  expect(result).toEqual(expectedOutput);
  expect(mockService.method).toHaveBeenCalledWith(inputData);
});
```

### 3. 錯誤測試

```typescript
it('should throw error when invalid input', async () => {
  mockService.method.mockResolvedValue(null);
  
  await expect(service.methodUnderTest(invalidInput))
    .rejects.toThrow('錯誤訊息');
});
```

## 常見問題解決

| 問題 | 解決方案 |
|------|----------|
| ESM 模組錯誤 | 使用 `ts-jest/presets/default-esm` |
| .js 匯入錯誤 | 設定 `moduleNameMapper` 映射 |
| Cannot redefine property | 將 `jest.mock()` 放在檔案最上方 |
| 同步/異步混淆 | 確認方法類型，使用對應的 mock 方法 |
| Mock 狀態污染 | 在 `beforeEach` 中使用 `jest.clearAllMocks()` |

## 測試策略

### 測試優先順序

1. **高價值**：核心業務邏輯、認證授權、資料驗證
2. **中價值**：資料轉換、邊界條件處理
3. **低價值**：簡單的 CRUD 操作、getter/setter

### 覆蓋重點

- 成功情境的主要流程
- 錯誤處理和邊界條件
- 重要的業務規則驗證

## 測試品質與過擬合防範

### 🚨 過擬合問題識別

#### 常見過擬合徵象

- **硬編碼數據過多**：固定的 ID、帳號、密碼格式
- **實現細節耦合**：測試依賴具體的資料庫欄位結構
- **缺乏數據變化**：使用相同的測試數據，沒有隨機性

```typescript
// ❌ 過擬合範例
const mockUser = {
  id: 1,                                    // 硬編碼ID
  account: "testAccount",                   // 硬編碼帳號
  passwordHash: "$2b$10$validHashedPassword", // 硬編碼hash格式
  profile: {
    nickname: "TestUser",                   // 硬編碼暱稱
    avatar: { public_id: "testAvatar" }     // 硬編碼頭像ID
  }
}

// 過度具體的斷言
expect(result.token).toBe("testJwtToken");
```

### ✅ 測試品質策略

#### 1. 使用工廠函數生成測試數據

```typescript
// 好的做法：彈性測試數據生成
const createMockUser = (overrides = {}) => ({
  id: Math.floor(Math.random() * 1000),
  account: `user_${Date.now()}`,
  passwordHash: `$2b$10$${Math.random().toString(36)}`,
  profile: {
    nickname: `User${Math.random().toString(36).substr(2, 5)}`,
    avatar: { public_id: `avatar_${Date.now()}` }
  },
  ...overrides
});

// 使用範例
const mockUser = createMockUser({ account: "specificTestAccount" });
```

#### 2. 測試行為而非數據結構

```typescript
// ✅ 好的測試：關注行為和約定
expect(mockJwtService.sign).toHaveBeenCalledWith(
  expect.objectContaining({ 
    userId: expect.any(Number),
    account: expect.any(String)
  })
);

// ✅ 驗證業務邏輯約定
expect(result).toEqual(
  expect.objectContaining({
    token: expect.any(String),
    account: mockUser.account,
    nickname: mockUser.profile.nickname
  })
);
```

#### 3. 屬性基礎測試 (Property-Based Testing)

```typescript
// 測試不同長度和格式的輸入
const testAccounts = [
  "user123",
  "test@example.com", 
  "a".repeat(50),
  "用戶名",
];

testAccounts.forEach(account => {
  it(`should handle account: ${account}`, async () => {
    const loginDto = { account, password: "validPassword" };
    // 測試邏輯...
  });
});
```

## 學習進度

### 已掌握

- ✅ Jest + ESM + TypeScript 環境配置
- ✅ NestJS 測試模組設定和依賴 Mocking
- ✅ 同步與異步方法測試
- ✅ 錯誤場景測試
- ✅ AAA 測試結構

### 待學習

- 🔄 測試覆蓋率分析
- 🔄 整合測試和 E2E 測試
- 🔄 進階 Mock 技巧和測試優化

## 參考範例

實際測試檔案：`src/modules/user/user.service.spec.ts`

- 包含成功和失敗情境的完整測試
- 可作為其他服務測試的模板
