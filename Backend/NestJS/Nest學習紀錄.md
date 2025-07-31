# NestJS學習紀錄筆記

本筆記濃縮鐵人賽文章，原文請查看: [NestJS 帶你飛](https://ithelp.ithome.com.tw/m/users/20119338/ironman/3880)

[TOC]

## 簡介

運用大量設計模式與架構規範，搭配強行別TypeScript，達到嚴謹、易擴展、鬆耦合。
![20119338F13oVCWfer](https://hackmd.io/_uploads/Sy-GWqaNJl.png)

NestJS採用模組化設計，將每種功能打包成獨立模組，且設計許多抽象層來將各個不同職責的程式碼片段抽成各式元件，具有高度的解耦合與擴展性。

NestJS 可以選擇使用 Express 或 Fastify 作為底層基礎，來打造 MVC 或 REST API 的應用，並將各種熱門套件進行整合。

NestJS結合三種程式設計概念:

- OOP
- FP
- FRP(函式反應式程式設計Functional Reactive Programming)

## 基本概念

Nest 採用模組化設計，將各個不同的功能區塊打包成 模組 (Module)，並且是以 樹狀結構 發散出去，最頂部的 Module 稱為 根模組 (Root Module)。

以Router機制為例，模組內會有Controller與Service，可將Service注入(Inject)到 Controller中使用，把處理請求的操作交給Controller，把處理邏輯的部分交給Service。
![image](https://hackmd.io/_uploads/ByvUB9641g.png)

## 安裝與解析範例專案

```bash
npm install -g @nestjs/cli
nest new <APP_NAME>
```

### 載入點

- `main.ts`

### 根模組

- `app.module.ts`

> 在 Nest 中，大部分的元件都是使用 裝飾器 (Decorator) 的方式來提供 元數據 (Metadata)

- `app.controller.ts`

> 註冊於根模組底下的 Controller， **不是必要項目**。

- `app.service.ts`

> 註冊於根模組底下的 Service，**不是必要項目**。

## 控制器(Controller)

Controller 負責路由的配置並處理來自客戶端的請求，而每一個 Controller 都可以依照需求來設計不同 Http Method 的資源。

![image](https://hackmd.io/_uploads/HJqu4CCEJe.png)

### 建置控制器

所有的 Controller 都必須使用 @Controller 裝飾器來定義。
使用NestCLI建立Controller:

```bash
nest generate controller <CONTROLLER_NAME>
```

可以看到src中多了兩個Controller檔案，並且在app.module中引入此Controller，而在新增的Controller檔案中會看到`@Controller(CONTROLLER_NAME)`，其中CONTROLLER_NAME為路由的[前綴(prefix)](/Of96lMkJTq2-u-5d42iG4Q#路由前綴(prefix))。

![image](https://hackmd.io/_uploads/BJSxi0AVJx.png)

### 裝飾器

#### @Request()

>請求的裝飾器，帶有此裝飾器的參數會賦予底層框架的 請求物件 (Request Object)，該裝飾器有別稱 `@Req()`，通常將參數名稱取為 `req`。

#### @Response()

>回應的裝飾器，帶有此裝飾器的參數會賦予底層框架的 回應物件 (Response Object)，該裝飾器有別稱 `@Res()`，通常將參數名稱取為 `res`。

#### @Next()

>Next 函式的裝飾器，帶有此裝飾器的參數會賦予底層框架的 Next 函式，用途為呼叫下一個 中介軟體 (Middleware)。

#### @Param(key?: string)

>路由參數的裝飾器，相當於 `req.params` 或 `req.params[key]`。

#### @Query(key?: string)

>查詢參數的裝飾器，相當於 `req.query` 或 `req.query[key]`。

#### @Body(key?: string)

>主體資料的裝飾器，相當於 `req.body` 或 `req.body[key]`。

#### @Headers(name?: string)

>請求標頭的裝飾器，相當於 `req.headers` 或 - `req.headers[name]`。

#### @Session()

>session 的裝飾器，相當於 `req.session`。

#### @Ip()

>IP 的裝飾器，相當於 `req.ip`。

#### @HostParam()

>host 的裝飾器，相當於 `req.hosts`。

## 模組(Module)

主要是把相同性質的功能包裝在一起，並依照各模組的需求來串接，而前面有提過整個 Nest App 必定有一個根模組，Nest 會從根模組架構整個應用。

### 建置 Module

所有的 Module 都必須使用 `@Module` 裝飾器來定義。可以用 NestCLI 快速生成 Module：

```bash
nest generate module <MODULE_NAME>
```

### Module參數介紹

#### controllers

>將要歸納在該 Module 下的 Controller 放在這裡，會在載入該 Module 時實例化它們。

#### providers

>將會使用到的 Provider 放在這裡，比如說：Service。會在載入該 Module 時實例化它們。

#### exports

>在這個 Module 下的部分 Provider 可能會在其他 Module 中使用，此時就可以把這些 Provider 放在這裡進行匯出。

#### imports

>將其他模組的 Provider 匯入。

### 功能模組 (Feature Module)

大多數的 Module 都屬於功能模組，其概念就是前面一直強調的：把相同性質的功能包裝在一起。
如果有模組需要使用到其他模組下的服務，可以透過`export`匯出和`import`匯入:
![image](https://hackmd.io/_uploads/ryLF1UkSye.png)

### 全域模組 (Global Module)

只需要在 Module 上再添加一個 `@Global` 的裝飾器，就能讓其他模組不需要匯入也能夠使用
但這部是一個好的設計模式，非必要請勿使用。

```typescript
import { Module, Global } from '@nestjs/common';

@Global()
@Module({})

export class ExampleModule {}
```

### 常用模組 (Common Module)

這是一種設計技巧，Module 可以不含任何 Controller 與 Provider，只單純把匯入的 Module 再匯出，以把多個常用的 Module 集中在一起。

```typescript
@Module({
  imports: [
    AModule,
    BModule
  ],
  exports: [
    AModule,
    BModule
  ],
})
export class CommonModule {}
```

## 提供者 (Provider)

>Provider 透過控制反轉容器做實例的管理

### 標準 Provider

在 class 上添加 `@Injectable` 讓 Nest 知道這個 class 是可以由控制反轉容器管理的。

```bash
nest generate service <SERVICE_NAME>
```

### 自訂 Provider

如果覺得標準 Provider 無法滿足需求，如：

- 想自行建立一個實例，而不是透過 Nest 建立。
- 想要在其他依賴項目中重用實例。
- 使用模擬版本的 class 進行覆寫，以便做測試。

#### Value Provider

>- 提供常數 (Constant)。
>- 將外部函式庫注入到控制反轉容器。
>- 將 class 抽換成特定的模擬版本。

#### Class Provider

>這類型的 Provider 最典型的用法就是讓 token 指定為抽象類別，並使用 `useClass` 來根據不同環境提供不同的實作類別。

#### Factory Provider

>這類型的 Provider 使用工廠模式讓Provider更加靈活，透過 注入其他依賴 來變化出不同的實例，是很重要的功能。使用`useFactory`來指定工廠模式的函數，並透過inject來注入其他依賴。
>主要特點
>
>- 可以根據環境變數或其他配置動態生成值
>- 可以執行複雜的初始化邏輯
>- 可以注入其他服務作為工廠函數的參數

#### Alias Provider

>這個 Provider 主要就是替已經存在的 Provider 取別名，使用 `useExist` 來指定要使用哪個 Provider。

#### 非同步 Provider

>有時候可能需要等待某些非同步的操作來建立 Provider，比如：需要與資料庫連線，Nest App 會等待該 Provider 建立完成才正式啟動。

#### 非類別型token

>Provider 的 token 不一定要使用 class，Nest 允許使用以下項目：
>
>- `string`
>- `symbol`
>- `enum`

## 例外處理 (Exception) & Exception filters

![image](https://hackmd.io/_uploads/rJs9xDkHkx.png)

### 標準 Exception

Nest 內建的標準 Exception 即為 `HttpException`

```typescript
  @Get()
  getHello(): string {
    throw new HttpException('出錯囉!', HttpStatus.BAD_REQUEST);
    return this.appService.getHello();
  }
```

如果不想使用預設格式，可以在第一個參數填入obj，Nest 會自動覆蓋格式。

Nest 有內建一套基於 `HttpException` 的 Exception 可以使用 : [Built-in HTTP exceptions](https://docs.nestjs.com/exception-filters#built-in-http-exceptions)

## Pipe

Pipe 經常被用來處理使用者傳入的參數，比如：驗證參數的正確性、型別的轉換等。
![image](https://hackmd.io/_uploads/SJzj4PkrJg.png)

Nest 內建了以下幾個 Pipe 來輔助資料轉型與驗證：

- `ValidationPipe`：驗證資料格式的 Pipe。
- `ParseIntPipe`：解析並驗證是否為 Integer 的 Pipe。
- `ParseBoolPipe`：解析並驗證是否為 Boolean 的 Pipe。
- `ParseArrayPipe`：解析並驗證是否為 Array 的 Pipe。
- `ParseUUIDPipe`：解析並驗證是否為 UUID 格式的 Pipe。
- `DefaultValuePipe`：驗證資料格式的 Pipe。

### DTO 技巧

當系統越來越龐大的時候，DTO 的數量也會隨之增加，有許多的 DTO 會有重複的屬性，例如：相同資源下的 CRUD DTO，這時候就會變得較難維護。

#### 局部性套用 (Partial)

>局部性套用的意思是將既有的 DTO 所有欄位都取用，只是全部轉換為非必要屬性，需要使用到 `PartialType` 這個函式來把要取用的 DTO 帶進去，並給新的DTO繼承。

```typescript
export class UpdateTodoDto extends PartialType(CreateTodoDto) {}
```

#### 選擇性套用 (Pick)

>選擇性套用的意思是用既有的 DTO 去選擇哪些是會用到的屬性，需要使用到`PickType`這個函式來把要取用的 DTO 帶進去以及指定要用的屬性名稱，並給新的DTO繼承。

```typescript
export class UpdateTodoDto extends PickType(CreateTodoDto,  ['title']) {}
```

#### 忽略套用 (Omit)

>忽略套用的意思是用既有的 DTO 但忽略不會用到的屬性，需要使用到 `OmitType` 這個函式來把要取用的 DTO 帶進去以及指定要忽略的屬性名稱，並給新的DTO繼承。

```typescript
export class UpdateTodoDto extends OmitType(CreateTodoDto,  ['title']) {}
```

#### 合併套用 (Intersection)

>合併套用的意思是用既有的兩個 DTO 來合併屬性，需要使用到 `IntersectionType` 這個函式來把要取用的兩個 DTO 帶進去，並給新的DTO繼承。

```typescript
export class UpdateTodoDto extends IntersectionType(CreateTodoDto, MockDto) {}
```

### 全域 Pipe

ValidationPipe 算是一個蠻常用的功能，可以直接將 ValidationPipe 配置在全域。

```typescript
import { APP_PIPE } from '@nestjs/core';
...

@Module({
  imports: [],
  controllers: [],
  providers: [
    {
      provide: APP_PIPE,
      useClass: ValidationPipe
    }
  ],
})
```

## 中介層 (Middleware)

Middleware 是一種執行於路由處理之前的函式，可以存取請求物件與回應物件，並透過 next() 繼續完成後續的流程，比如說：執行下一個 Middleware、進入正式的請求資源流程。

![image](https://hackmd.io/_uploads/BJd4zXlBye.png)

- 可執行任何程式。
- 更改請求物件或回應物件。
- 結束整個請求週期。
- 呼叫下一個執行步驟。
- 如果在 Middleware 沒有結束掉請求週期，需要使用 next() 呼叫下一個執行步驟。

### 設計 Middleware

Middleware 有兩種設計方式，一般的 function 或 帶有 @Injectable 裝飾器並實作 NestMiddleware 介面的 class

#### Functional middleware

>這種 Middleware 十分單純，就是一個普通的 function，不過有三個參數，分別是：Request、Response 以及 NextFunction。

```typescript
import { Request, Response, NextFunction } from 'express';

export function logger(req: Request, res: Response, next: NextFunction) {}
```

>適合較為簡單的邏輯檢查。

#### Class middleware

>這種 Middleware 可以透過 CLI 產生:

```bash
nest generate middleware <MIDDLEWARE_NAME>
```

>適合使用在需要依賴注入和複雜邏輯時。

## 攔截器 (interceptor)

為原功能的擴展邏輯

![image](https://hackmd.io/_uploads/ryd_breHkl.png)

### 設計Interceptor

可以透過 CLI 產生

```bash
nest generate interceptor <INTERCEPTOR_NAME>
```

#### CallHandler

>`CallHandler` 為Interceptor的重要成員，它實作了`handle()`來調用路由處理的方法，進而導入對應的 Controller 方法。
>也就是說，如果在 Interceptor不回傳 CallHandler 的 `handle()`，將會使路由處理失去運作。
>用簡單的來說，它的作用就是去呼叫後面的function，並回傳處理後的數值或物件。
>而由於`handle()`會回傳處理的物件，所以可以透過Pipe的方式對回傳值做調整。

```typescript
//這段程式碼展示資料是如何流向以及Interceptor的基本機制

@Injectable()
export class ExmapleInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler) {
    console.log('1. 開始處理請求'); // 第一個執行點

    //這邊會呼叫下一個需要執行的物件或函式
    return next.handle().pipe(
      map(data => {
        console.log('3. 處理 Controller 返回的資料');
        return {
            message: '可以修改回傳的物件'
        };
      }),

      tap(data => {
        console.log('4. 觀察處理後的資料，但不修改');
        // 這裡可以做日誌記錄
      })
    );
  }
}

//
export class ExmapleController {
  @Get()
  Exmaple() {
    console.log('2. Controller 方法執行中');
    return {};
  }
}

```

>上面這段程式碼在控制台會顯示:
>
>1. 開始處理請求
>2. Controller 方法執行中
>3. 處理 Controller 返回的資料
>4. 觀察處理後的資料，但不修改

##### Observable（可觀察物件）

>這邊的`handle()`是一種Observable，Observable 是一種特殊的資料流，它代表的意思是**未來某個時間點會產生的資料**。
>把它作為intercept的回傳值是希望Nest去subscribe它，根據Observable的特性，若沒有去 subscribe則不會執行其內部邏輯，這也是不回傳`handle()`的話將會使路由處理失去運作的原因。

#### ExecutionContext

>`ExecutionContext` 是繼承 ArgumentsHost 的 class，其提供了更多關於此請求的相關訊息。

## 守衛 (Guard)

經常用在身份驗證與授權，當有未經授權的請求時，將會由 Guard 攔截並擋下。
與其他框架常使用的守衛觀念差不多。

![image](https://hackmd.io/_uploads/BkUXELxrJx.png)

### 設計 Guard

```bash
nest generate guard <GUARD_NAME>
```

## 裝飾器 (Decorator)

他是一種設計模式，白話來說就是用來描述類別或物件的提示字。

![image](https://hackmd.io/_uploads/ryEX2LgBJg.png)

### 客製 Decorator

在某些情況下內建的裝飾器可能沒辦法很有效地解決問題，於是 Nest 提供了 自訂裝飾器 (Custom Decorator) 的功能

### 設計裝飾器

```bash
nest generate decorator <DECORATOR_NAME>
```

#### 參數裝飾器

>有些資料可能無法透過內建裝飾器直接取得，比如：授權認證機制所帶入的資料。假如在中介層中對請求加入自定義的資料，`req.user='PoleEast'`，可以透過自定義的裝飾器直接取出。

```typescript
//這是一個參數裝飾器，data會是呼叫時傳入的參數，ctx為上下文
export const User = createParamDecorator(
  (data: unknown, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    //這邊將在中介層中加入的user資料提取出來，放到所裝飾的參數上
    return request.user;
  },
);

//透過User取出在中介層定義的user
@Get('/decoratorTemple')
 getDecoratorTemple(@User() user: string) {
    return { message: `裝飾器範本，user為${user}` };
 }
```

#### Metadata 裝飾器

>有時候需要針對某個方法設置特定的 Metadata，比如：角色權限控管，透過設置 Metadata 來表示該方法僅能由特定角色來存取。

```typescript!
//這是一個 Metadata 裝飾器
export const Roles = (...args: string[]) => SetMetadata('roles', args);
```

>為了要取得 Metadata 的內容，
>必須透過 Nest 提供的 Reflector 來取得，
>其引入方式即透過依賴注入，
>並呼叫 `get(metadataKey: any, target: Function | Type<any>)` 來取得指定的 Metadata，
>其中 `metadataKey` 即要指定的 Metadata Key，
>而 `target` 則為裝飾器裝飾之目標，
>經常會使用 ExecutionContext 裡面的 `getHandler` 來作為 `target` 的值。

```typescript!
//在RoleGuard使用reflector來獲取裝飾器的標籤
const roles = this.reflector.get<string[]>('roles', context.getHandler());

//最後在controller增加此裝飾器
@Controller()
export class AppController {
  constructor() {}

  @UseGuards(RoleGuard)
  @Roles('admin')
  @Get()
  getHello(@User('name') name: string): string {
    return name;
  }
```

>可以把這個過程想像成在一本書上貼便利貼：
>
>- `@Roles('admin')` 就像是寫了一張便利貼 (Metadata)
>- `SetMetadata` 就像是把這張便利貼貼到特定頁面（方法）上
>- `Reflector` 就像是一個能夠讀取這些便利貼的工具
>- `context.getHandler()` 告訴 `Reflector` 要看哪一頁
>- `ROLES_KEY` 告訴 `Reflector` 要找什麼顏色的便利貼

#### 整合裝飾器

>有些裝飾器它們之間是有相關的，每次要實作都要重複將這些裝飾器帶入，會使得重複性的操作變多，可以使用 `applyDecorators` 的函式來將多個裝飾器整合成一個裝飾器，每當要實作該功能就只要帶入整合裝飾器即可。

```typescript!
//可以將多個裝飾器整合成一個裝飾器
export const Auth = (...roles: string[]) => applyDecorators(
    Roles(...roles),
    UseGuards(AuthGuard, RoleGuard)
);
```

## Injection Scopes

Nest 在大多數情況下是採用 單例模式 (Singleton) 來維護各個實例，也就是說，各個進來的請求都共享相同的實例，這些實例會維持到 Nest App 結束為止。

Nest.js 的依賴注入（DI）系統有三種作用域（scope）：

### 預設作用域 (Default scope)

`共享且不會因請求改變狀態`

### 請求作用域 (Request scope)

`需要追蹤單個請求生命週期`

為每個請求建立全新的實例，在該請求中的 Provider 是共享實例的，請求結束後將會進行垃圾回收。

#### Request應用情境

>- 請求追蹤日誌
>- 用戶身份驗證狀態
>- 請求特定的計時器
>- 單個請求的數據收集
>- API 響應時間監控

### 獨立作用域 (Transient scope)

`每次使用都需要全新狀態`

每個 Provider 都是獨立的實例，在各 Provider 之間不共享。

#### Transient應用情境

>- 文件上傳處理器（每個上傳需要獨立的狀態）
>- 獨立的資料轉換器
>- 每次都需要新建的工具類
>- 臨時數據處理器
>- 支付處理器（每筆交易都需要獨立的狀態）

### 作用域冒泡

在 NestJS 中，當一個組件（比如 Controller）依賴於一個非單例（non-singleton）的提供者時，它本身也會變成非單例的。

![image](https://hackmd.io/_uploads/HyTZsLSSJx.png)

如果我們把StorageService的作用域設置為「請求作用域」，那麼依賴於 StorageService 的BookService與AppService都會變成請求作用域。

## 生命週期鉤子 (Lifecycle hook)

- Module 初始化階段 (onModuleInit)
- Nest App 啟動階段 (onApplicationBootstrap)
- Module 銷毀階段 (onModuleDestroy)
- Nest App 關閉前 (beforeApplicationShutdown)
- Nest App 關閉階段 (onApplicationShutdown)

## 模組參照 (Module Reference)

允許你在運行時獲取模組的引用，主要用於動態獲取模組內的 providers。

```typescript!
// payment.strategy.ts
interface PaymentStrategy {
  process(amount: number): Promise<void>;
}

@Injectable()
class StripeStrategy implements PaymentStrategy {
  async process(amount: number) {}
}

@Injectable()
class PayPalStrategy implements PaymentStrategy {
  async process(amount: number) {}
}

// payment.service.ts
@Injectable()
class PaymentService {
  constructor(private moduleRef: ModuleRef) {}

  async processPayment(type: 'stripe' | 'paypal', amount: number) {
    // 根據類型動態獲取支付策略
    const strategyClass = type === 'stripe' ? StripeStrategy : PayPalStrategy;
    const strategy = await this.moduleRef.resolve(strategyClass);
    
    return strategy.process(amount);
  }
}
```

### get、resolve、create

`get`、`resolve` 和 `create` 三者的差別

```typescript
@Injectable()
class UserService {
  constructor(private configService: ConfigService) {
    console.log('UserService created');
  }
}

@Controller()
class AppController {
  constructor(private moduleRef: ModuleRef) {}

  async someMethod() {
    // 1. get: 同步獲取現有實例
    const service1 = this.moduleRef.get(UserService);
    // - 如果是 Singleton scope，返回已存在的實例
    // - 如果是 REQUEST/TRANSIENT scope，會拋出錯誤
    // - 同步方法（不是 async）

    // 2. resolve: 異步獲取或創建實例
    const service2 = await this.moduleRef.resolve(UserService);
    // - 可以處理所有 scope（Singleton/Request/Transient）
    // - 每次調用都可能創建新實例（根據 scope）
    // - 異步方法

    // 3. create: 強制創建新實例
    const service3 = await this.moduleRef.create(UserService);
    // - 總是創建新實例
    // - 完全脫離依賴注入系統
    // - 異步方法
  }
}
```
