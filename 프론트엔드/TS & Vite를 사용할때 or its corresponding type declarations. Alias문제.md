2023-12-08
### 왜 오류가 발생 하는가 ?
TypeScript(TS)와 Vite를 함께 사용할 때는 두 도구 모두에서 경로 별칭(alias) 설정을 별도로 해주어야 합니다. 이는 TypeScript가 JavaScript의 타입 안정성을 높이기 위한 컴파일러 역할을 하고, Vite가 프론트엔드 프로젝트의 빌더 역할을 수행하기 때문입니다.

오늘 2시간 가까이 삽질하며 알아낸 점은, Spring Boot에서 사용하는 Gradle이나 Maven과는 다르게, TypeScript와 Vite는 서로 경로 별칭을 공유하지 않는다는 것입니다. Spring Boot에서는 빌드 관리 도구가 의존성 관리와 빌드, 배포를 담당하며, 설정이 분리되어 있습니다. 반면, TypeScript와 Vite는 각각 독립적으로 설정을 관리해야 하며, 특히 경로 별칭 설정이 중복되어 필요합니다.

이러한 차이점은 TypeScript와 Vite가 다루는 역할과 범위에서 기인합니다. TypeScript는 주로 코드 컴파일과 타입 체킹에 집중하고, Vite는 모듈 번들링과 개발 서버 실행 등의 작업을 처리합니다. 따라서 두 도구 모두에서 경로 별칭을 설정해 주어야 하는 것입니다.

Vite 설정

```ts
import { defineConfig } from 'vite'  
import vue from '@vitejs/plugin-vue'  
  
export default defineConfig({  
  plugins: [vue()],  
  resolve: {  
    alias: {  
      '@': new URL('./src', import.meta.url).pathname  
    }  
  }  
})
```

ts.config 설정
```json
{  
  "compilerOptions": {  
    "target": "ES2020",  
    "useDefineForClassFields": true,  
    "module": "ESNext",  
    "lib": ["ES2020", "DOM", "DOM.Iterable"],  
    "skipLibCheck": true,  
  
    /* Bundler mode */  
    "moduleResolution": "node",  
    "allowImportingTsExtensions": true,  
    "resolveJsonModule": true,  
    "isolatedModules": true,  
    "noEmit": true,  
    "jsx": "preserve",  
  
    /* Linting */  
    "strict": true,  
    "noUnusedLocals": true,  
    "noUnusedParameters": true,  
    "noFallthroughCasesInSwitch": true,  
    "baseUrl": ".",  
    "paths": {  
      "@/*": ["./src/*"]  
    }  
  },  
  "include": ["src/**/*.ts", "src/**/*.tsx", "src/**/*.vue"],  
  "references": [{ "path": "./tsconfig.node.json" }]  
}
```