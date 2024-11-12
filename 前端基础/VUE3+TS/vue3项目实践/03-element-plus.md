## 安装
```node
npm install element-plus --save
```

## 按需引入

### 自动导入
安装`unplugin-vue-components` 和 `unplugin-auto-import`这两款插件

```node
npm install -D unplugin-vue-components unplugin-auto-import
```

```node
// vite.config.ts
import { defineConfig } from 'vite'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default defineConfig({
  // ...
  plugins: [
    // ...
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
})
```

## 全局配置

```node
// App.vue
import ElementPlus from 'element-plus'

app.use(ElementPlus, { size: 'small', zIndex: 3000 })
```


## 自定义命名空间

### 设置ElConfigProvider

```vue
<template>
  <el-config-provider namespace="ep">
    <!-- ... -->
  </el-config-provider>
</template>
```

### 设置 SCSS 和 CSS 变量

创建 `styles/element/index.scss`：

```scss
// styles/element/index.scss
// we can add this to custom namespace, default is 'el'
@forward 'element-plus/theme-chalk/src/mixins/config.scss' with (
  $namespace: 'ep'
);
// ...
```

在 `vite.config.ts` 中导入 `styles/element/index.scss`：
```ts
import { defineConfig } from 'vite'
// https://vitejs.dev/config/
export default defineConfig({
  // ...
  css: {
    preprocessorOptions: {
      scss: {
        additionalData: `@use "~/styles/element/index.scss" as *;`,
      },
    },
  },
  // ...
})
```

配置完别名之后，样式生效还需配置
```js
// vite.config.ts
AutoImport({
    imports: ['vue', 'vue-router'],
    dts: 'types/auto-imports.d.ts',
    resolvers: [ElementPlusResolver()],
  }),
  Components({
    // 这一行
    resolvers: [ElementPlusResolver({importStyle: 'sass'})],
    dts: 'types/components.d.ts'
  }),
```
## AutoImport配置

```ts
// vite.config.ts
import AutoImport from 'unplugin-auto-import/vite'
 
export default defineConfig({
  plugins: [
    AutoImport({
     imports: [
        'vue',
        'vue-router',
        'vue-i18n',
        '@vueuse/core',
        'pinia',
      ],
      dts: 'types/auto-imports.d.ts', // 使用typescript，需要指定生成对应的d.ts文件或者设置为true,生成默认导入d.ts文件
      dirs: ['src/stores', 'src/composables', 'src/hooks'],
    }),
  ],
})
```

[unplugin-auto-import](https://link.segmentfault.com/?enc=kb5V4DNHMSblqMILjv55UQ%3D%3D.Ep8SbjtJ3ep1IBF%2FYPBF%2BzTx33YiJcrKsAQEoC8LwFrW7KNSNjtujpSkuMsNwl9Oy2d4HgFe5IbSyqbZ4%2F6G%2Bw%3D%3D) 是能够按配置自动按需导入的插件，减少开发时需要不断 import 的问题

dirs: `目录下模块导出的自动导入`。默认情况下，它只扫描目录下的一级模块，我们可以添加一些通配符来做筛选,限定到指定目录，或者到src 都可以做到自动引入，但是为了精确化查找，建议匹配越精准越好
```js
 // 配置本地目录 自动引入
  dirs: ['./src/utils/**', './src/stores/**'],
 //dirs: ['./src/**'],

```

dts： 生成相应`.d.ts`文件的文件路径

imports: 需要全局引入的

参考 [项目中自动引入神器 - unplugin-auto-import/unplugin-vue-components-CSDN博客](https://blog.csdn.net/qq_38845858/article/details/137112474)