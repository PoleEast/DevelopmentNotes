# Canvas 2D API 基礎知識

## 概述

Canvas 2D API 是 HTML5 提供的原生圖形繪製 API，允許在網頁上動態繪製 2D 圖形、圖像和動畫。

## 基本概念

### Canvas 元素和上下文

```html
<canvas id="myCanvas" width="800" height="400"></canvas>
```

```javascript
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d'); // 獲取 2D 渲染上下文
```

### 座標系統

- 原點 (0,0) 位於左上角
- X 軸向右增加
- Y 軸向下增加
- 單位為像素

## 基本繪圖方法

### 1. 路徑繪製

```javascript
ctx.beginPath();        // 開始新路徑
ctx.moveTo(50, 50);     // 移動到指定點
ctx.lineTo(200, 50);    // 畫線到指定點
ctx.lineTo(125, 150);   // 繼續畫線
ctx.closePath();        // 關閉路徑
ctx.stroke();           // 描邊
ctx.fill();             // 填充
```

### 2. 基本圖形

```javascript
// 矩形
ctx.fillRect(10, 10, 100, 50);      // 填充矩形
ctx.strokeRect(10, 10, 100, 50);    // 描邊矩形
ctx.clearRect(10, 10, 100, 50);     // 清除矩形區域

// 圓形/弧形
ctx.arc(100, 100, 50, 0, 2 * Math.PI); // (x, y, radius, startAngle, endAngle)
ctx.fill();
```

### 3. 文字繪製

```javascript
ctx.font = '20px Arial';
ctx.fillStyle = 'blue';
ctx.fillText('Hello Canvas!', 50, 50);

// 文字屬性
ctx.textAlign = 'center';     // 'left', 'right', 'center', 'start', 'end'
ctx.textBaseline = 'middle';  // 'top', 'bottom', 'middle', 'alphabetic'
```

## 樣式和顏色

### 基本顏色設定

```javascript
ctx.fillStyle = '#FF0000';        // 填充顏色
ctx.strokeStyle = 'rgb(0,255,0)'; // 描邊顏色
ctx.lineWidth = 5;                // 線條寬度
```

### 漸變效果

#### 線性漸變

```javascript
const gradient = ctx.createLinearGradient(x0, y0, x1, y1);
gradient.addColorStop(0, '#FF0000');    // 起始顏色
gradient.addColorStop(0.5, '#00FF00');  // 中間顏色
gradient.addColorStop(1, '#0000FF');    // 結束顏色

ctx.fillStyle = gradient;
ctx.fillRect(0, 0, 200, 100);
```

#### 徑向漸變

```javascript
const radialGradient = ctx.createRadialGradient(x0, y0, r0, x1, y1, r1);
radialGradient.addColorStop(0, '#FF0000');
radialGradient.addColorStop(1, '#0000FF');

ctx.fillStyle = radialGradient;
ctx.fillRect(0, 0, 200, 200);
```

### 圖案填充

```javascript
const img = new Image();
img.onload = function() {
  const pattern = ctx.createPattern(img, 'repeat'); // 'repeat', 'repeat-x', 'repeat-y', 'no-repeat'
  ctx.fillStyle = pattern;
  ctx.fillRect(0, 0, 200, 200);
};
img.src = 'pattern.png';
```

## 變換

### 基本變換

```javascript
ctx.translate(100, 100);  // 平移
ctx.rotate(Math.PI / 4);  // 旋轉（弧度）
ctx.scale(2, 1.5);        // 縮放 (x, y)
```

### 變換矩陣

```javascript
ctx.transform(a, b, c, d, e, f);
// a: 水平縮放, b: 水平傾斜, c: 垂直傾斜, d: 垂直縮放, e: 水平平移, f: 垂直平移

ctx.setTransform(a, b, c, d, e, f); // 重置並設定變換
ctx.resetTransform(); // 重置為單位矩陣
```

### 保存和恢復狀態

```javascript
ctx.save();    // 保存當前狀態
// ... 進行變換和繪製
ctx.restore(); // 恢復之前保存的狀態
```

## 圖像處理

### 繪製圖像

```javascript
const img = new Image();
img.onload = function() {
  // 基本繪製
  ctx.drawImage(img, x, y);
  
  // 指定大小
  ctx.drawImage(img, x, y, width, height);
  
  // 裁剪和縮放
  ctx.drawImage(img, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
};
img.src = 'image.png';
```

### 像素操作

```javascript
// 獲取像素資料
const imageData = ctx.getImageData(x, y, width, height);
const data = imageData.data; // Uint8ClampedArray [R, G, B, A, R, G, B, A, ...]

// 修改像素
for (let i = 0; i < data.length; i += 4) {
  data[i] = 255;     // Red
  data[i + 1] = 0;   // Green  
  data[i + 2] = 0;   // Blue
  data[i + 3] = 255; // Alpha
}

// 放回像素資料
ctx.putImageData(imageData, x, y);
```

## 動畫

### 基本動畫循環

```javascript
function animate() {
  ctx.clearRect(0, 0, canvas.width, canvas.height); // 清除畫布
  
  // 繪製動畫幀
  draw();
  
  requestAnimationFrame(animate); // 請求下一幀
}

animate();
```

### 使用時間控制動畫

```javascript
let lastTime = 0;

function animate(currentTime) {
  const deltaTime = currentTime - lastTime;
  lastTime = currentTime;
  
  // 使用 deltaTime 更新動畫狀態
  update(deltaTime);
  render();
  
  requestAnimationFrame(animate);
}

requestAnimationFrame(animate);
```

## 事件處理

### 滑鼠事件

```javascript
canvas.addEventListener('click', function(event) {
  const rect = canvas.getBoundingClientRect();
  const x = event.clientX - rect.left;
  const y = event.clientY - rect.top;
  
  console.log(`點擊位置: (${x}, ${y})`);
});

canvas.addEventListener('mousemove', function(event) {
  const rect = canvas.getBoundingClientRect();
  const x = event.clientX - rect.left;
  const y = event.clientY - rect.top;
  
  // 處理滑鼠移動
});
```

## 最佳實踐

### 1. 性能優化

```javascript
// 避免頻繁的狀態變更
ctx.save();
ctx.fillStyle = 'red';
// 批次繪製相同樣式的物件
ctx.restore();

// 使用 Path2D 重用路徑
const circlePath = new Path2D();
circlePath.arc(50, 50, 20, 0, 2 * Math.PI);
ctx.fill(circlePath);
```

### 2. 設定 Canvas 尺寸

```javascript
// 考慮設備像素比
const dpr = window.devicePixelRatio || 1;
canvas.width = canvas.clientWidth * dpr;
canvas.height = canvas.clientHeight * dpr;
ctx.scale(dpr, dpr);
```

### 3. 錯誤處理

```javascript
// 檢查瀏覽器支持
if (!canvas.getContext) {
  console.error('Canvas not supported');
  return;
}

const ctx = canvas.getContext('2d');
if (!ctx) {
  console.error('Could not get 2D context');
  return;
}
```

## 常用工具函數

```javascript
// 角度轉弧度
function toRadians(degrees) {
  return degrees * Math.PI / 180;
}

// 弧度轉角度
function toDegrees(radians) {
  return radians * 180 / Math.PI;
}

// 距離計算
function distance(x1, y1, x2, y2) {
  return Math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2);
}

// 隨機顏色
function randomColor() {
  return `hsl(${Math.random() * 360}, 50%, 50%)`;
}
```

## 總結

Canvas 2D API 提供了強大的圖形繪製能力，是許多圖表庫（如 Chart.js）的底層基礎。掌握這些基本概念和方法，有助於理解和擴展圖表庫的功能，也能直接創建自定義的圖形應用。