# auto-preview-image
HengTuo auto-preview-image.

## Installation
```
npm install auto-preview-image --save
```

## Import
```
在main.js中
import 'auto-preview-image/dist/auto-preview-image.min.css';
import { ApImage } from 'auto-preview-image';
```
```
Vue.use(ApImage);
```

使用

```
<ap-image :showClose="false" :thumb="imgUrl" :radius="3" :full="imgUrl" :maxWidth="300"></ap-image>
```

## Changelog
### 2018.8.26
> v0.1.4 * 初始化组件库
