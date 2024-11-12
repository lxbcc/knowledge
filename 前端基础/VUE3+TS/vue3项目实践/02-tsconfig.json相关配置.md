[TypeScript配置-- 2. 了解ts配置项，根据vite项目了解typescript配置文件，tsconfig.json、tsconfig.node.json、-CSDN博客](https://blog.csdn.net/cs492934056/article/details/132514195)
## tsconfig.json

根目录下的tsconfig.json是根文件,没有配置，引入了其他文件
```json
{
  "files": [],
  "references": [
    {
      "path": "./tsconfig.app.json"
    },
    {
      "path": "./tsconfig.node.json"
    }
  ]
}
```

## tsconfig.node.json

从`tsconfig.node.json` 的 **include** 可以看出，这个主要是限制配置文件的配置文件，比如vite.config.ts 的配置。
```json
{
  "compilerOptions": {
    "composite": true,
    "tsBuildInfoFile": "./node_modules/.tmp/tsconfig.node.tsbuildinfo",
    "skipLibCheck": true,
    "module": "ESNext",
    "moduleResolution": "bundler",
    "allowSyntheticDefaultImports": true,
    "noEmit": true
  },
  "include": ["vite.config.ts"]
}
```

## tsconfig.app.json
```json
{
  "compilerOptions": {
    "composite": true,
    "tsBuildInfoFile": "./node_modules/.tmp/tsconfig.app.tsbuildinfo",
    "target": "ES2020",
    "useDefineForClassFields": true,
    "module": "ESNext",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "moduleDetection": "force",
    "noEmit": true,
    "jsx": "preserve",

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "baseUrl": "./", // 解析非相对模块的基地址，默认是当前目录
    "paths": { //路径映射，相对于baseUrl
      "@/*": ["src/*"] 
    },
    "types": ["element-plus/global"]
  }
}
```

## tsconfig.vitest.json
配置单元测试


>提示
>1. 如果在 tsconfig.json 中没有配置 “files”、“include” 或 “exclude”，但是有 “compilerOptions”，则 TypeScript译器会默认编译当前目录下的所有 TypeScript 文件（以 .ts 或 .tsx 扩展名结尾的文件）。
>2. 1. 如果通过**references**引入了多个项目，并且项目都指明了file\include\exclude 同一个源文件，将会引用最后一个references的文件。
>3. 1. 如果通过**references**引入了2个项目，第一个配置文件**tsconfig.app.json**，涵盖了 **env.d.ts**文件；第二个配置文件tsconfig.vitest.json文件默认所有文件，则会执行**tsconfig.app.json**配置。
>



## 相关配置
[tsconfig.json配置项备忘-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2366913)
[TypeScript: TSConfig Reference - Docs on every TSConfig option](https://www.typescriptlang.org/tsconfig/)