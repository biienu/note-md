# es6模块化

## es6模块引入 

![image-20220722002344189](D:/ProgramFiles/typora/typora-images/image-20220722002344189.png)

***

![image-20220722002631464](D:/ProgramFiles/typora/typora-images/image-20220722002631464.png)

***

![image-20220722002711154](D:/ProgramFiles/typora/typora-images/image-20220722002711154.png)



## es6模块暴露

![image-20220722002022155](D:/ProgramFiles/typora/typora-images/image-20220722002022155.png)

***

![image-20220722002238670](D:/ProgramFiles/typora/typora-images/image-20220722002238670.png)

***

![image-20220722002301292](D:/ProgramFiles/typora/typora-images/image-20220722002301292.png)

***



# JavaScript   的JSON.stringify() 转化为json对象



# es6方法中的参数可以设置默认值

![image-20220723155509349](D:/ProgramFiles/typora/typora-images/image-20220723155509349.png)



# vue过滤器 

![image-20220723155905439](D:/ProgramFiles/typora/typora-images/image-20220723155905439.png)



# 函数式指令

![image-20220723194638849](D:/ProgramFiles/typora/typora-images/image-20220723194638849.png)

![image-20220723194902319](D:/ProgramFiles/typora/typora-images/image-20220723194902319.png)



## 指令中的 this 是 window 而不是 vm



# Vue 生命周期



# Vue脚手架

![image-20220724092216235](D:/ProgramFiles/typora/typora-images/image-20220724092216235.png)





# props 传的参数不能修改

![image-20220724102757535](D:/ProgramFiles/typora/typora-images/image-20220724102757535.png)

**如果传的是一个对象，则可以修改对象中属性的值，但不能修改这个对象。**



# 混入mixin

![image-20220724105434277](D:/ProgramFiles/typora/typora-images/image-20220724105434277.png)

**全局引入混入**:

![image-20220724110158700](D:/ProgramFiles/typora/typora-images/image-20220724110158700.png)



# 插件

**定义插件：**

install(Vue)中的Vue参数是Vue构造器

![image-20220724110438440](D:/ProgramFiles/typora/typora-images/image-20220724110438440.png)

**使用插件：**

![image-20220724110655005](D:/ProgramFiles/typora/typora-images/image-20220724110655005.png)

***

**插件总结**：

![image-20220724111613162](D:/ProgramFiles/typora/typora-images/image-20220724111613162.png)



# scoped样式

![image-20220724113109628](D:/ProgramFiles/typora/typora-images/image-20220724113109628.png)

***

# webstorage(localStorage, sessionStorage)

![image-20220724182534598](D:/ProgramFiles/typora/typora-images/image-20220724182534598.png)

***

# 事件

## 定义事件

**使用ref属性**：

![image-20220724193503708](D:/ProgramFiles/typora/typora-images/image-20220724193503708.png)

## 事件解绑

**解绑一个事件**：

![image-20220724192403099](D:/ProgramFiles/typora/typora-images/image-20220724192403099.png)



**解绑多个事件**

![image-20220724192506502](D:/ProgramFiles/typora/typora-images/image-20220724192506502.png)

**解绑所有的事件**

![image-20220724192601861](D:/ProgramFiles/typora/typora-images/image-20220724192601861.png)



## 事件总结

![image-20220724194426000](D:/ProgramFiles/typora/typora-images/image-20220724194426000.png)

***



# 全局事件总线

![image-20220724195132402](D:/ProgramFiles/typora/typora-images/image-20220724195132402.png)



**定义全局事件总线**：

![image-20220724201145649](D:/ProgramFiles/typora/typora-images/image-20220724201145649.png)



**全局事件总线总结**：

![image-20220724201521085](D:/ProgramFiles/typora/typora-images/image-20220724201521085.png)

# vue中组件间传值的方法：

## 父-> 子    props

## 子 -> 父 :

1. 自定义事件
2. props，父给子传递一个函数，当子调用这个函数时，可以通过函数传递给父。



# 消息发布与订阅

![image-20220724204552212](D:/ProgramFiles/typora/typora-images/image-20220724204552212.png)

****

***

***





***

![image-20220724204358906](D:/ProgramFiles/typora/typora-images/image-20220724204358906.png)

![image-20220724204348062](D:/ProgramFiles/typora/typora-images/image-20220724204348062.png)



# nextTick

![image-20220724205331940](D:/ProgramFiles/typora/typora-images/image-20220724205331940.png)

***



# 插槽



# Vuex

*Vuex4版本只能在Vue3版本使用，Vuex3版本在Vue2版本中使用。*



**原理图**:

![image-20220806084200475](D:/ProgramFiles/typora/typora-images/image-20220806084200475.png)

Actions中的函数接收的(arg1,arg2) arg1是上下文对象，arg2是接收到的value值，





**使用**：

![image-20220724234633622](D:/ProgramFiles/typora/typora-images/image-20220724234633622.png)

![](D:/ProgramFiles/typora/typora-images/image-20220806192727580.png)

![](D:/ProgramFiles/typora/typora-images/image-20220806192636565.png)

## vuex模块化

**简写：**

![image-20220806083606962](D:/ProgramFiles/typora/typora-images/image-20220806083606962.png)

![image-20220806085329542](D:/ProgramFiles/typora/typora-images/image-20220806085329542.png)



**手写：**

![image-20220806085342105](D:/ProgramFiles/typora/typora-images/image-20220806085342105.png)



![image-20220806084053853](D:/ProgramFiles/typora/typora-images/image-20220806084053853.png)



## 模块化

![image-20220806085530967](D:/ProgramFiles/typora/typora-images/image-20220806085530967.png)

![image-20220806085636348](D:/ProgramFiles/typora/typora-images/image-20220806085636348.png)



# vue-router

*vue-router@3 在vue2版本使用, vue-router@4在vue3版本使用*

![image-20220725093823961](D:/ProgramFiles/typora/typora-images/image-20220725093823961.png)

![image-20220725094542064](D:/ProgramFiles/typora/typora-images/image-20220725094542064.png)



![](D:/ProgramFiles/typora/typora-images/image-20220725094321222.png)

![image-20220725094414124](D:/ProgramFiles/typora/typora-images/image-20220725094414124.png)

## 几个注意点：

![image-20220725095335922](D:/ProgramFiles/typora/typora-images/image-20220725095335922.png)



## 多级路由：

![image-20220725130458151](D:/ProgramFiles/typora/typora-images/image-20220725130458151.png)



![image-20220804153449604](D:/ProgramFiles/typora/typora-images/image-20220804153449604.png)



![image-20220725130706250](D:/ProgramFiles/typora/typora-images/image-20220725130706250.png)



![image-20220725130753037](D:/ProgramFiles/typora/typora-images/image-20220725130753037.png)

![image-20220725130805974](D:/ProgramFiles/typora/typora-images/image-20220725130805974.png)



## 路由参数



### query参数：

![](D:/ProgramFiles/typora/typora-images/image-20220725132334611.png)

### params参数

![](D:/ProgramFiles/typora/typora-images/image-20220725133426391.png)

![image-20220725133408132](D:/ProgramFiles/typora/typora-images/image-20220725133408132.png)

### props

![image-20220725134552887](D:/ProgramFiles/typora/typora-images/image-20220725134552887.png)

## 命名路由

![image-20220725132747486](D:/ProgramFiles/typora/typora-images/image-20220725132747486.png)

![image-20220725132810133](D:/ProgramFiles/typora/typora-images/image-20220725132810133.png)

##  route-link的 replace属性

![image-20220725135145329](D:/ProgramFiles/typora/typora-images/image-20220725135145329.png)

## 编程式导航路由

![image-20220725140944215](D:/ProgramFiles/typora/typora-images/image-20220725140944215.png)

## 缓存路由组件 

`include的值为组件名字，而非路由名子`

![image-20220725141520401](D:/ProgramFiles/typora/typora-images/image-20220725141520401.png)

## 路由守卫

### 全局路由守卫



![image-20220725144943998](D:/ProgramFiles/typora/typora-images/image-20220725144943998.png)

### 独享路由守卫

写在router.js的具体的路由中

![image-20220725145259547](D:/ProgramFiles/typora/typora-images/image-20220725145259547.png)



### 组件内路由守卫

写在组件内

![image-20220725150251124](D:/ProgramFiles/typora/typora-images/image-20220725150251124.png)



## 路由的两种工作方式：

![image-20220725151942409](D:/ProgramFiles/typora/typora-images/image-20220725151942409.png)





# vue中两个新的生命周期钩子



![image-20220725142237466](D:/ProgramFiles/typora/typora-images/image-20220725142237466.png)





# 常见的ui组件库

![image-20220725153918068](D:/ProgramFiles/typora/typora-images/image-20220725153918068.png)

![image-20230508201507839](D:\ProgramFiles\Typora\typora-images\image-20230508201507839.png)
