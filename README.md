# Create React App to Vite

## Start CRA not work on IE11

```shell
$ yarn create react-app cra-to-vite --template typescript
$ cd cra-to-vite/
$ yarn start
```

Access http://localhost:3000


## CRA work on IE11

Add code first line of `src/index.tsx`.

```typescript
/** @jsxRuntime classic */
import 'react-app-polyfill/ie11';
```

Add `package.json`'s browserslist.

```diff
    "development": [
+     "ie 11"
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
```

```shell
$ yarn start
```

Access http://localhost:3000


## CRA to Vite not work on IE11

Fix `package.json`.

```diff
+   "devDependencies": {
+     "@vitejs/plugin-react-refresh": "^1.3.6",
+     "vite": "^2.5.5"
+   },
+   "scripts": {
-     "start": "react-scripts start",
-     "build": "react-scripts build",
-     "test": "react-scripts test",
-     "eject": "react-scripts eject"
+     "start": "vite",
+     "build": "vite build",
+     "serve": "vite preview"
+   },
```

Create `vite.config.ts`.

```typescript
import { defineConfig } from 'vite'
import reactRefresh from '@vitejs/plugin-react-refresh'

export default defineConfig({
  build: {
    outDir: 'build',
  },
  plugins: [
    reactRefresh(),
  ],
})
```

Fix `tsconfig.json`

```diff
-     "allowJs": true,
-     "skipLibCheck": true,
-     "esModuleInterop": true,
+     "types": [
+       "vite/client"
+     ],
+     "allowJs": false,
+     "skipLibCheck": false,
+     "esModuleInterop": false,
```

Move `public/index.html` to project root directory

Then, remove `%PUBLIC_URL%` and add `script` tag in `body`.

```html
<script type="module" src="/src/index.tsx"></script>
```

```shell
$ yarn start
```

Access http://localhost:3000


## Vite work on IE11

Fix `package.json`.

```diff
    "devDependencies": {
      "@vitejs/plugin-react-refresh": "^1.3.6",
+     "@vitejs/plugin-react-refresh": "^1.3.6",
      "vite": "^2.5.5"
    },
```

Fix `vite.config.ts`

```diff
  import { defineConfig } from 'vite'
+ import reactRefresh from '@vitejs/plugin-react-refresh'
  import reactRefresh from '@vitejs/plugin-react-refresh'

  export default defineConfig({
    build: {
      outDir: 'build',
    },
    plugins: [
      reactRefresh(),
+     legacy({
+       targets: ['ie >= 11'],
+       additionalLegacyPolyfills: ['regenerator-runtime/runtime']
+     })
    ],
  })
```


```shell
$ yarn build
$ yarn serve
```

Access http://localhost:5000
