### unplugin-auto-import (自动导入)

[vue3+ts+vite项目使用 unplugin-auto-import (自动导入)\_vite autoimport-CSDN博客](https://blog.csdn.net/halo1416/article/details/136541343)
[vite + vue + ts 自动按需导入 Element Plus组件，并如何解决按需引入后ElMessage与ElLoading 的问题（找不到名称“ElMessage”问题。）-CSDN博客](https://blog.csdn.net/weixin_59916662/article/details/127334196)

项目中会频繁引入使用 `vue、vue-router、pinia` 内的API， 如`ref、reactive` 等  
使用自动引入插件进行全局引入即可

- 安装：`npm i -D unplugin-auto-import`
- 修改 `vite.config.ts`

```js
import AutoImport from 'unplugin-auto-import/vite'

export default defineConfig({
  plugins: [
    // ... other
    AutoImport({
      imports: ['vue','vue-router', 'pinia'], // 自动引入的三方库
      dts: 'src/types/auto-import.d.ts', // 全局自动引入文件存放路径；不配置保存在根目录下；配置为false时将不会生成 auto-imports.d.ts 文件（不影响效果）
      eslintrc: {
        // 项目中使用了 eslint，那么虽然可以正常使用 API 了，但是 eslint 还是会报没有引入的报错。下面的配置可以处理这种情况
        enabled: true, // 会在根目录下自动生成 .eslintrc-auto-import.json 文件
      }
    })
  ]
})

```

另见 [[03-element-plus#AutoImport配置]]

