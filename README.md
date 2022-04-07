# yiqing
vue移动端项目-疫情实时检测
项目初始化
1.安装创建项目 vue create vue-yiqing
2.安装：npm i axios vant -S
3.删除无用组件
4.css初始化
5.移动端适配方案：引入flexible.js+rem布局
vm布局   100vw=750px
6.在src文件夹下面配置一个公共的接口文档


处理接口地址
1.http://api.tianapi.com/ncov/index?key=7e0778b6421f2d33bd11ac75810b6f0b

出行疫情防疫政策接口：（聚合数据）
https://www.juhe.cn/docs/api/id/566
注册并使命认证

2.安装vant库
npm i vant -S
自动按需组件：npm i babel-plugin-import -D
对于使用babel7的用户，可以在babel.config.js中配置
module.exports={
 "plugins": [
    [
      "import",
      {
        "libraryName": "vant",
        "libraryDirectory": "es",
        "style": true
      }
    ]
  ]
};
创建一个vant.js
在main.js中引入vant 库   import'@/plugins/vant.js'

全局注册组件：Vue.use()
当前页面注册组件：component:{
VanButton:Button       //即<van-button></van-button>
}
或者[Button.name]:Button

echarts
安装echarts
npm install echarts@4.8 -S
使用方式：
1.方式1：组件内部使用单独使用
● 导入：import echarts from 'echarts'
● 直接使用
2.方式2：挂载到vue原型上所有组件都可以使用
● 导入：import echarts from 'echarts'
● main.js 挂载 Vue.prototype.$echarts=echarts
3.方式3：开发成vue组件

第三方库和生命周期产生影响，无法获取库里面的dom元素
this.$nextTick()  可以延迟中数据的影响
等待延迟加载
第三方库是异步的，按需加载


md5加密
md5加密规则：sign = MD5( appid1formatjsontime1545829466密钥) 查看加密规则说明 密钥不需要键名，请直接跟上32位的密钥
    //步骤：1. 安装 npm i md5-js -S  2. 引入md5模块  3. 使用加密处理
    // sign = MD5( appid1formatjsontime1545829466密钥)

swpier使用
1.swiper官网
npm install swiper@5.x vue-awesome-swiper --save
在mian.js中导入文件
1.
import VueAwesomeSwiper from 'vue-awesome-swiper'
import 'swiper/css/swiper.css'
Vue.use( VueAwesomeSwiper)


组件使用
<template>
  <swiper ref="mySwiper" :options="swiperOptions">
    <swiper-slide>Slide 1</swiper-slide>
    <swiper-slide>Slide 2</swiper-slide>
    <swiper-slide>Slide 3</swiper-slide>
    <div class="swiper-pagination" slot="pagination"></div>
  </swiper>
</template>
export default {
  data() {
    return {
      //swiper配置内容
      swiperOptions: {
        pagination: {
          el: ".swiper-pagination",
        },
        autoplay: {
          delay: 3000,
          stopOnLastSlide: false,
          disableOnInteraction: false,
        },
        loop: true,
      },
    };
  },
  computed: {
    //获取swiper实例对象
    swiper() {
      return this.$refs.mySwiper.$swiper;
    },
  },
  mounted() {
   console.log('swiper',this.swiper)
    this.swiper.slideTo(3, 1000, false);
  },
};
</script>

<style lang='less' scoped>
.list {
  display: flex;
  li {
    flex: 1;
    padding: 0.1rem;
    font-size: 0.24rem;
    line-height: 0.32rem;
    margin: 0.1rem;
    background: #eee;
    color: #666;
    text-align: center;
    display: flex;
    align-items: center;
  }
  .active {
    background: rgb(80, 116, 173);
    color: #fff;
  }
}
</style>

出行政策接口：
 travel:'http://apis.juhe.cn/springTravel/citys?key=171e165a7d991c5f6ecd5194c54778ef'

这个接口有跨域问题，所以使用proxy代理解决跨域问题
在根目录下设置vue.config.js  并修改配置项

module.exports = {
  devServer: {
    proxy: {
      '/fpp': {
        secure: false,
        target: 'http://apis.juhe.cn', //聚合接口地址
        ws: true,
        changeOrigin: true,
        pathRewrite: {
          //重写路径
          '^/fpp': ''
        }
      }
    }
  }
}


for in 遍历对象和Object.keys()  遍历键名
 this.citylist = res.data.result.citylist
         for(let key in this.citylist ){
          this.indexList.push(key)

         }
         console.log(this.indexList)


    // Object.keys(obj) 获取所有的键名 --[] 
    //  this.indexList = Object.keys(res.data.result.citylist)
    //  console.log(this.indexList);

用到this.$router.push('')跳转首页时，如果之前页面又过滑动的时候，现在新跳转的不会回到顶部，借助js document.documentElement.scrollTop=0;可以回到首页。

 this.$router.go(-1);不会有随着前一页滑动的问题，每次跳转都是回到首页
     this.$router.push('/')   //首页
      document.documentElement.scrollTop=0;
    //  this.$router.go(-1);

vue组件数据传递方式
1.父传子：props   自定义属性
2.子传父：this.$emit()   自定义事件
兄弟元素：子组件数据给父组件--- 另外的子组件
3.父组件实例：$parent
4.子组件实例：$children
5.dom元素和子组件实例：$refs
6.获取跟组件： $root
7.深层传递：provide/inject
8.原型链传递：vue.prototype.xx=xxx
9.本地存储：localStorge.setItem()/getItem()
10.eventbus  中央系统  兄弟之间传递  跨组件之间传递数据

父子组件嵌套：
生命周期函数：beforeCreate-- created --beforeMount--子组件4个（beforeCreate-- created --beforeMount-Mounted）
---父组件的Mounted



eventBus 中央系统- 跨组件之间传递数据
1,定义eventBus.js
2.创建eventBus.js
import Vue from 'vue'
const Bus =new Vue();
export default Bus
3.main.js引入eventBus.js
import Bus from './eventBus.js'
Vue.prototype.$bus=Bus
4.适用语法：
this.$bus.$emit('sun','hahaha');
接受数据：
this.$bus.$on('sun',val=>{
val
};
缺点：不支持响应式  ，维护起来很难

Eventbus项目遇到坑：
1.a页面跳转到b页面，b页面里面事件触发返回a页面，并携带b页面的参数返回，随即想到eventbus
b页面触发的方法
  jump(event)
     // 返回上一层
      this.$router.push('/')   //首页
      document.documentElement.scrollTop=0;

    this.$bus.$emit('city',event.target.innerHTML)

    }
  },
a页面进行监听
 created() {
  //   // 1.获取本地存储
  //   //  let city=localStorage.getItem('city')
  //   // 2.获取eventbus
     this.$bus.$on('city', val => {
     console.log('--val--', val)
       this.city = val+'疫情';
  
  //   })
结果在a页面发现val是可以接收到的，但a页面的数据却没有发生改变
问题：
● 问题1： 为什么第一次触发的时候页面B中的on事件没有被触发
● 问题2： 为什么后面再一次依次去触发的时候会出现，每一次都会发现好像之前的on事件分发都没有被撤销一样，导致每一次的事件触发执行越来越多。
此时发现在加载新组件时，在新组件挂载之前会销毁上一个组件，然后挂载新的组件.
我自己做了实验来验证，这个页面跳转过程中，这两个组件的生命周期的执行情况。

当从页面A到页面B跳转的时候，发生了什么？

其实，可以通过结果清楚看到，当我们还在页面A的时候，页面B还没生成，也就是页面B中的 created中所监听的来自于A中的事件还没有被触发。这个时候当你A中emit事件的时候，B其实是没有监听到的。
再看一下，红色的是B页面组件，当你从页面A到页面B跳转的时候，发生了什么？首先是先B组件先created然后beforeMount接着A组件才被销毁，A组件才执行beforeDestory，以及destoryed.
所以，我们可以把A页面组件中的emit事件写在beforeDestory中去。因为这个时候，B页面组件已经被created了，也就是我们写的$on事件已经触发了


此时，应该b组件销毁之前传递数据

b页面应该将数据在beforeDestory阶段或者destoryed阶段传递
  beforeDestroy(){
    this.$bus.$emit('city', this.city)
  }
a页面此时就可以渲染数据了
但是，好像，就是事件的触发还是会依次增加，就是控制台的输出每次都有所增加了。。。
*就是说，这个$on事件是不会自动清楚销毁的，需要我们手动来销毁。
所以。我在a组件页面中添加Bus.$off来关闭。代码如下：
  beforeDestroy(){
    this.$bus.$off('city')
  },
