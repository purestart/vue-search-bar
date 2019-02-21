# vue-search-bar

#### 项目介绍

Vue 组件，搜索框、带搜索页面的搜索框

![image](https://gitee.com/_pure/codes/dn4u2bw65zxemfklcao9i45/raw?blob_name=github-vue-search-bar.gif)

#### 软件架构

软件架构说明

#### 安装教程

    npm install vue-search-bar --save

#### 使用说明

    在main.js中
    import 'vue-search-bar/dist/vue-search-bar.min.css';
    import { SearchBar } from 'vue-search-bar';
    Vue.use(SearchBar);

    页面中使用
    <search-bar :onChange="onChange" :onSearch="onSearch">
        搜索页面 推荐标签 搜索提示等等...
    </search-bar>

#### 参与贡献

1. Fork 本项目
2. 新建 Feat_xxx 分支
3. 提交代码
4. 新建 Pull Request
