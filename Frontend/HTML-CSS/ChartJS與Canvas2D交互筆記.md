# Chart.js 與 Canvas 2D 交互筆記

## 架構關係

### 整體架構圖
```
Vue 組件 (使用者介面層)
      ↓
Vue-ChartJS (Vue 包裝器)
      ↓
Chart.js (圖表邏輯層)
      ↓
Canvas 2D API (底層渲染)
      ↓
HTML Canvas Element (DOM 元素)
```

### 關係說明

1. **Chart.js** 是建立在 Canvas 2D API 之上的圖表庫
2. **Vue-ChartJS** 是 Chart.js 的 Vue 包裝器
3. Chart.js **暴露 Canvas 上下文**，讓開發者可以直接使用 Canvas 2D API

## Chart.js 如何使用 Canvas 2D

### 1. 基本渲染流程

```javascript
// Chart.js 內部流程（簡化）
class Chart {
  constructor(ctx, config) {
    this.ctx = ctx; // Canvas 2D 上下文
    this.canvas = ctx.canvas; // Canvas 元素
  }

  render() {
    // 1. 清除畫布
    this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
    
    // 2. 繪製背景
    this.drawBackground();
    
    // 3. 繪製網格線
    this.drawGrid();
    
    // 4. 繪製資料
    this.drawDatasets();
    
    // 5. 繪製標籤、圖例等
    this.drawLabels();
  }
}
```

### 2. Chart.js 座標轉換

Chart.js 提供資料值與像素座標的轉換：

```javascript
// 資料值轉像素座標
const pixelX = chart.scales.x.getPixelForValue(dataValue);
const pixelY = chart.scales.y.getPixelForValue(dataValue);

// 像素座標轉資料值
const dataValue = chart.scales.x.getValueForPixel(pixelX);
```

## 在 Chart.js 中使用 Canvas 2D API

### 1. 動態顏色函數

這是最常見的用法，通過函數回傳 Canvas 漸變物件：

```javascript
const chartConfig = {
  datasets: [{
    borderColor: (context) => {
      const chart = context.chart;
      const { ctx, chartArea } = chart;
      
      if (!chartArea) return '#000'; // 初始化時的備用顏色
      
      // 使用 Canvas 2D API 創建漸變
      const gradient = ctx.createLinearGradient(
        chartArea.left, 0, 
        chartArea.right, 0
      );
      gradient.addColorStop(0, '#FF6B35');
      gradient.addColorStop(1, '#4A90E2');
      
      return gradient; // 回傳 CanvasGradient 物件
    }
  }]
};
```

### 2. 自定義背景

```javascript
const chartConfig = {
  datasets: [{
    backgroundColor: (context) => {
      const chart = context.chart;
      const { ctx, chartArea } = chart;
      
      if (!chartArea) return 'transparent';
      
      // 徑向漸變背景
      const centerX = (chartArea.left + chartArea.right) / 2;
      const centerY = (chartArea.top + chartArea.bottom) / 2;
      const radius = Math.min(chartArea.width, chartArea.height) / 2;
      
      const gradient = ctx.createRadialGradient(
        centerX, centerY, 0,
        centerX, centerY, radius
      );
      gradient.addColorStop(0, 'rgba(255, 107, 53, 0.8)');
      gradient.addColorStop(1, 'rgba(255, 107, 53, 0.1)');
      
      return gradient;
    }
  }]
};
```

### 3. Context 物件詳解

當 Chart.js 呼叫顏色函數時，會傳入 context 物件：

```javascript
borderColor: (context) => {
  // context 包含以下屬性：
  const {
    chart,        // Chart 實例
    dataIndex,    // 當前資料點索引
    dataset,      // 當前資料集
    datasetIndex  // 資料集索引
  } = context;
  
  // chart 物件包含：
  const {
    ctx,          // Canvas 2D 渲染上下文
    canvas,       // Canvas DOM 元素
    chartArea,    // 圖表可繪製區域 { top, left, bottom, right, width, height }
    scales,       // 軸縮放物件
    data,         // 完整的圖表資料
    options       // 圖表選項
  } = chart;
}
```

## 高級用法

### 1. 自定義圖案填充

```javascript
backgroundColor: (context) => {
  const { ctx } = context.chart;
  
  // 創建圖案畫布
  const patternCanvas = document.createElement('canvas');
  patternCanvas.width = 20;
  patternCanvas.height = 20;
  const patternCtx = patternCanvas.getContext('2d');
  
  // 繪製圖案
  patternCtx.fillStyle = '#FF6B35';
  patternCtx.fillRect(0, 0, 10, 10);
  patternCtx.fillRect(10, 10, 10, 10);
  
  patternCtx.fillStyle = '#4A90E2';
  patternCtx.fillRect(10, 0, 10, 10);
  patternCtx.fillRect(0, 10, 10, 10);
  
  // 創建圖案
  return ctx.createPattern(patternCanvas, 'repeat');
}
```

### 2. 基於資料的動態顏色

```javascript
pointBackgroundColor: (context) => {
  const { dataIndex, dataset } = context;
  const value = dataset.data[dataIndex];
  const { ctx, chartArea } = context.chart;
  
  if (!chartArea) return '#000';
  
  // 根據數值創建不同顏色
  const minValue = Math.min(...dataset.data);
  const maxValue = Math.max(...dataset.data);
  const normalizedValue = (value - minValue) / (maxValue - minValue);
  
  const gradient = ctx.createLinearGradient(0, 0, 0, chartArea.height);
  gradient.addColorStop(0, `hsl(${120 * normalizedValue}, 70%, 50%)`);
  gradient.addColorStop(1, `hsl(${120 * normalizedValue}, 70%, 30%)`);
  
  return gradient;
}
```

### 3. 動畫過程中的顏色變化

```javascript
borderColor: (context) => {
  const chart = context.chart;
  const { ctx, chartArea } = chart;
  
  if (!chartArea) return '#000';
  
  // 獲取動畫進度（0-1）
  const animationProgress = chart.currentStep / chart.numSteps;
  
  const gradient = ctx.createLinearGradient(
    chartArea.left, 0, 
    chartArea.right, 0
  );
  
  // 根據動畫進度調整顏色
  const startColor = `rgba(255, 107, 53, ${animationProgress})`;
  const endColor = `rgba(74, 144, 226, ${animationProgress})`;
  
  gradient.addColorStop(0, startColor);
  gradient.addColorStop(1, endColor);
  
  return gradient;
}
```

## 實際應用場景

### 1. 溫度圖表漸變（我們的例子）

```javascript
// 溫度從低到高的顏色漸變
borderColor: (context) => {
  const { ctx, chartArea } = context.chart;
  if (!chartArea) return '#000';
  
  const gradient = ctx.createLinearGradient(
    chartArea.left, 0, 
    chartArea.right, 0
  );
  gradient.addColorStop(0, '#4A90E2');  // 藍色（低溫）
  gradient.addColorStop(0.5, '#FFA726'); // 橘色（中溫）
  gradient.addColorStop(1, '#FF6B35');   // 紅色（高溫）
  
  return gradient;
}
```

### 2. 股票圖表（漲跌顏色）

```javascript
borderColor: (context) => {
  const { dataIndex, dataset } = context;
  const currentValue = dataset.data[dataIndex];
  const previousValue = dataset.data[dataIndex - 1];
  
  if (dataIndex === 0) return '#666';
  
  const { ctx, chartArea } = context.chart;
  if (!chartArea) return '#666';
  
  const isRising = currentValue > previousValue;
  const gradient = ctx.createLinearGradient(0, chartArea.top, 0, chartArea.bottom);
  
  if (isRising) {
    gradient.addColorStop(0, '#00C851'); // 綠色（上漲）
    gradient.addColorStop(1, '#007E33');
  } else {
    gradient.addColorStop(0, '#FF4444'); // 紅色（下跌）
    gradient.addColorStop(1, '#CC0000');
  }
  
  return gradient;
}
```

### 3. 時間區間漸變

```javascript
backgroundColor: (context) => {
  const { ctx, chartArea } = context.chart;
  if (!chartArea) return 'transparent';
  
  // 垂直時間漸變（例如：白天到夜晚）
  const gradient = ctx.createLinearGradient(0, chartArea.top, 0, chartArea.bottom);
  gradient.addColorStop(0, 'rgba(255, 255, 0, 0.3)');    // 黃色（白天）
  gradient.addColorStop(0.5, 'rgba(255, 165, 0, 0.2)');  // 橘色（黃昏）
  gradient.addColorStop(1, 'rgba(25, 25, 112, 0.3)');    // 深藍（夜晚）
  
  return gradient;
}
```

## 性能考量

### 1. 緩存漸變物件

```javascript
let cachedGradient = null;

borderColor: (context) => {
  const { ctx, chartArea } = context.chart;
  if (!chartArea) return '#000';
  
  // 只有在圖表區域改變時才重新創建漸變
  if (!cachedGradient || 
      cachedGradient.chartAreaWidth !== chartArea.width ||
      cachedGradient.chartAreaHeight !== chartArea.height) {
    
    const gradient = ctx.createLinearGradient(
      chartArea.left, 0, 
      chartArea.right, 0
    );
    gradient.addColorStop(0, '#FF6B35');
    gradient.addColorStop(1, '#4A90E2');
    
    cachedGradient = {
      gradient,
      chartAreaWidth: chartArea.width,
      chartAreaHeight: chartArea.height
    };
  }
  
  return cachedGradient.gradient;
}
```

### 2. 避免複雜計算

```javascript
// ❌ 避免在顏色函數中進行複雜計算
borderColor: (context) => {
  // 避免複雜的數學運算或大量迴圈
  const expensiveCalculation = someComplexFunction();
  return generateGradient(expensiveCalculation);
}

// ✅ 預先計算，在顏色函數中使用結果
const preCalculatedValues = someComplexFunction();

borderColor: (context) => {
  return generateGradient(preCalculatedValues);
}
```

## 除錯技巧

### 1. 檢查 chartArea 是否存在

```javascript
borderColor: (context) => {
  const { ctx, chartArea } = context.chart;
  
  // 在初始化階段，chartArea 可能是 undefined
  if (!chartArea) {
    console.log('Chart area not ready');
    return '#000'; // 回傳備用顏色
  }
  
  console.log('Chart area:', chartArea);
  // 繼續處理...
}
```

### 2. 驗證漸變建立

```javascript
borderColor: (context) => {
  const { ctx, chartArea } = context.chart;
  if (!chartArea) return '#000';
  
  const gradient = ctx.createLinearGradient(
    chartArea.left, 0, 
    chartArea.right, 0
  );
  
  // 檢查漸變物件
  console.log('Gradient created:', gradient instanceof CanvasGradient);
  
  gradient.addColorStop(0, '#FF6B35');
  gradient.addColorStop(1, '#4A90E2');
  
  return gradient;
}
```

## 總結

Chart.js 與 Canvas 2D 的交互讓我們能夠：

1. **使用 Canvas 原生功能**：創建漸變、圖案、複雜視覺效果
2. **動態顏色計算**：基於資料、動畫進度或其他條件
3. **高度客製化**：超越 Chart.js 預設的樣式選項
4. **性能控制**：通過理解底層機制優化渲染效能

關鍵是理解 Chart.js **暴露了 Canvas 上下文**，讓開發者可以直接使用 Canvas 2D API 的所有功能，這就是為什麼我們可以在 `borderColor` 等屬性中使用函數回傳 `CanvasGradient` 物件的原因。