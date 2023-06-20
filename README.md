# CI/CD Pipeline

![upload_e90e8b02e2af43c762d3dc7b992a79d1](https://github.com/chienniman/mantine-vite-template/assets/97031067/dc136230-7215-4ddf-890c-a902e49305e7)

## How to use
* git clone
```
https://github.com/chienniman/mantine-vite-template.git
```

* download the ZIP


![upload_3bcee1509ead2f2ec62f4d8d748e7c40](https://github.com/chienniman/mantine-vite-template/assets/97031067/2779e096-1b2e-4d34-b422-5a8cfe86cb1f)


* fork the project


![upload_5f373696b2f8b7a8eff1b80ac74e7915](https://github.com/chienniman/mantine-vite-template/assets/97031067/4aff9a1d-f264-42b7-8539-893e442332d8)


## Create CI/CD workflow

`
.github
/workflows/main.yml
`
```YML
name: Deploy to gh-page
permissions:
  contents: write
on:
  push:
    branches: master

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Setup Node
        uses: actions/setup-node@v2

      - name: CI
        run: npm ci

      - name: Typecheck
        run: npm run typecheck

      - name: Prettier
        run: npm run prettier
      
      - name: Lint
        run: npm run lint

      - name: Jest
        run: npm run jest

      - name: Run build
        run: npm run build
      
      - name: Upload production-ready build files
        uses: actions/upload-artifact@v2
        with:
          name: production-files
          path: ./dist

  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/master'

    steps:
    - name: Download artifact
      uses: actions/download-artifact@v2
      with:
        name: production-files
        path: ./dist

    - name: Deploy to gh-pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist          
```

## Local development and usage

### install yarn

```bash
yarn --version
```

```bash
npm install --global yarn
```

### install node_modules
```
yarn install
```

### add homepage to package.json
```
"homepage": "https://<githubusername>.github.io/<app>"
```

### Start the vite server and develop new features.
```
npm run dev
```

### test
```
npm run test
```

## Commit to master branch

```
git init
git add .
git commit -m "xxx"
```

## Enable GitHub Pages
The source must be set to gh-pages instead of master/main, otherwise it will not be displayed.


![upload_4e2f51163291505fef5b6963ba124a24](https://github.com/chienniman/mantine-vite-template/assets/97031067/56a59957-be4d-4cc7-99f0-d1f0dbd4f53f)


## Done
![upload_52e440e776779864c9b295a4f58e150f](https://github.com/chienniman/mantine-vite-template/assets/97031067/1179e108-301c-44be-a25b-adc5cbc3d791)


## Discussion

### Assets links
To fix the issue of resource `loading 404 errors` and modify the default path, you can achieve it through the configuration in vite.config.ts.

`vite.config.ts`

```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  base: '/mantine-vite-template/',
  build: {
    assetsDir: 'assets',
  },
});

```
### Pages build and deployment
> I think there's a lot of scope for confusion for users of this action, when pages build and deployment will also be running on every push and pushing pages assets.

I think it's impossible to really say right now what that looks like until GitHub announces what these actions intend to do in the long run. As the post suggests this is a necessary step that occurs after the push gets made, this was already occurring behind the scenes it just wasn't made visible.




## Ref
* [yarn](https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable)
* [What is actions/checkout@v2 in Github action](https://github.com/orgs/community/discussions/25682)
* [deploy-to-github-pages](https://github.com/marketplace/actions/deploy-to-github-pages)
* [vite-deploy-demo](https://github.com/sitek94/vite-deploy-demo)
* [antonputra/tutorials](https://github.com/antonputra/tutorials/tree/main/lessons/100)
* [github-pages-deploy-action](https://github.com/JamesIves/github-pages-deploy-action/discussions/1073)

