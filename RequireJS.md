https://github.com/requirejs/requirejs
http://requirejs.cn/
http://www.ruanyifeng.com/blog/2012/11/require_js.html
http://www.runoob.com/w3cnote/requirejs-tutorial-1.html

```
require.config({
　　　　shim: {

　　　　　　'underscore':{
　　　　　　　　exports: '_'
　　　　　　},
　　　　　　'backbone': {
　　　　　　　　deps: ['underscore', 'jquery'],
　　　　　　　　exports: 'Backbone'
　　　　　　}
　　　　}
　　});
```

define()
```
模块的依赖
define(['myLib'], function(myLib){
　　　　function foo(){
　　　　　　myLib.doSomething();
　　　　}
　　　　return {
　　　　　　foo : foo
　　　　};
　　});
```