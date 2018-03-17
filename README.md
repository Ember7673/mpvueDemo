# my-project

> A Mpvue project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

#mpvue 初试美团开源小程序

### mpvue中dist是小程序的工作目录

### mpvue不支持class对象和class数组
#### 一般class绑定应用于点击高亮  移动端hover事件相当于 web端的active事件 代替达到高亮效果

### 解决定时器问题  
#### 定时器再全局定义时可以在任何作用域中清除  局部作用域中定义的只能在本作用域中清除定时器

### mpvue 不支持transtion  实现动画有点不太方便

