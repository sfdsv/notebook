多层画布的关键在于

1. position绝对，
2. z-index分布，数字小的在下层

实现代码：

```css

<style>

  canvas {

   position: absolute;

   border: 1px solid #000000;

  }

  .canvas-bottom{

   z-index: 0;

   background-color: aliceblue;

  }

  .canvas-top{

   z-index: 1;

   background-color: blue;

  }
  
 </style>
```

