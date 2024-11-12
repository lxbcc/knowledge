[Vite 项目中如何去集成 Mock 环境 （插件：vite-plugin-mock）-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2420352)
### 安装
安装插件
```js
pnpm install -D vite-plugin-mock mockjs
```
配置vite.config.ts
```ts
import {viteMockServe} from "vite-plugin-mock";

export default defineConfig(({ command }) => {
	return {
		plugins:[
			vue(),
			viteMockServe({
				enable: command === 'serve' // 保证项目开发阶段可以使用 mock 接口 
			})
		]
	}
})
```

之后在项目根目录创建 mock 文件夹，去创建我们需要的 Mock 数据和对应的接口