## index.html

进入项目前的加载效果

![[Pasted image 20240729165045.png|152]]

```html
// html
<div class="love-loading">
	<img class="love-loading-img" src="/vite.svg" alt="logo">
	<div class="love-loading-title">fle-template-ui</div>
	<div class="love-loading-text">Loading...</div>
</div>
<script type="module" src="/src/main.ts"></script>
```

vue的public的资源在打包时不会被编译，只会copy

所以在在src路径下引入public文件夹下的图片、视频、音频，编译不会改变其路径，但是在src下引入public文件夹下的js、json，在打包时都会被编译，所以直接引入会丢失路径（因为打包时，当前页面引入的路径被hash打包，而public文件夹下只是被copy）故需要再src下引入public文件夹的资源需要根据情况进行处理

在引入图片类资源 （img标签src属性赋值绝对路径/）
```html
<div class="view"><img src="/moon.png" /></div>
```

在引入js、json文件时，需要全局引入

页面入口index.html 在head中引入js文件
```html
<head>
    <script type="module" src="/ep.js"></script>
    ....
</head>
```
### 垂直居中
```css
.love-loading {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate3d(-50%, -50%, 0);
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
```

`transform:translate3d(x,y,z)`: 其中x、y、z分别指要移动的轴的方向的距离，三个参数不能省略，没有就写0
*需要链接*
### 动画效果
```css
.love-loading-text {
    font-size: 26px;
    letter-spacing: 2px;
    font-family: Arial, Helvetica, sans-serif;
    color: #2a64f6;
    animation: loading-animation 1s ease-in infinite alternate;
  }

  @keyframes loading-animation {
    0% {
      filter: blur(0);
      transform: skew(0deg);
    }

    100% {
      filter: blur(3px);
      transform: skew(-4deg);
    }
  }
```

**animation**
- animation-name：设置需要绑定到元素的动画名称；
- animation-duration：设置完成动画所需要花费的时间，单位为秒或毫秒，默认为 0；
- animation-timing-function：设置动画的速度曲线，默认为 ease；
- animation-fill-mode：设置当动画不播放时（动画播放完或延迟播放时）的状态；
- animation-delay：设置动画开始之前的延迟时间，默认为 0；
- animation-iteration-count：设置动画被播放的次数，默认为 1；
- animation-direction：设置是否在下一周期逆向播放动画，默认为 normal；
- animation-play-state：设置动画是正在运行还是暂停，默认是 running；
- animation：所有动画属性的简写属性。
**@keyframes**
CSS3中添加的新属性animation是用来为元素实现动画效果的，但是animation无法单独担当起实现动画的效果。承载动画的另一个属性——@keyframes。使用的时候为了兼容可加上-webkit-、-o-、-ms-、-moz-、-khtml-等前缀以适应不同的浏览器。


## App.vue

```vue
  <el-config-provider
    namespace="love"
    size="default"
  >
    <router-view />
  </el-config-provider>
```


## Login.vue
