### 介绍
mptoast 是一个基于mpvue的简单弹窗组件  github地址：[https://github.com/noahlam/mpvue-toast](https://github.com/noahlam/mpvue-toast)

### 特性
0. **轻量** 目前整个项目`未打包前`大概只有`120行`代码(包括注释)，5kb左右(包括图标)
1. **配置少**  尝试过无数种优化方法，只为减少配置
2. **冗余少**  每个页面（page）只需要引入一次，该页面里面如果有多个子组件，可以跟页面共用一个，无需重复引入。
3. **使用简单** 除了必须的在page页面对组件import，注册，和html引入（这些麻烦的东西由于mpvue不支持的原因，暂时无法做到优化），其他的使用只需一行简单的代码 `this.$mptoast('提示消息‘)`即可实现弹窗
4. **可定制性强** 提供用户重写样式的属性，只需传入一个定义好的`样式类名`既可实现对原有样式的覆盖（具体请看参数说明）


### 安装

1.安装`vuex`，如果你项目还没使用的话。请放心，虽然`mptoast`依赖`vuex`,你不会接触到任何有关`vuex`的代码。添加`vuex`只为让你写更少的代码。

    npm i vuex

2.安装`mptoast`

    npm i mptoast -D

或者

    yarn add mptoast --dev

3.在项目的主配置文件`（一般位于src/main.js）`加入以下代码

    import mpvueToastRegistry from 'mptoast/src/registry'
    mpvueToastRegistry(Vue)

4.在你需要弹窗的页面，引入组件，并注册，然后在页面内加入一个你注册的组件，就可以在js里面调用`this.$mptoast()`了， 以下是一个简单的实例

    <template>
      <div>
        <-- 省略其他代码 -->
        <mptoast />
      </div>
    </template>

    <script>
    import mptoast from 'mptoast'

    export default {
      components: {
        mptoast
      },
      data () {
        return {}
      },
      methods: {
        showToast () {
          this.$mptoast('我是提示信息')
        },
      }
    }
    </script>

至于为什么没办法做到像vue组件那样，引入一次，就可以在所有页面使用，我想我必须得解释以下，因为mpvue目前还不支持全局的组件，我尝试过很多种变通办法，都行不通，甚至为了让大家使用的时候，少输入几个字，少一些冗余，我都做了很多尝试和优化，目前mpvue团队已经在考虑新增全局组件功能，我会时刻关注，一旦支持，我这边也立马做支持。

### 参数说明
> 参数分2种类型，一种是多个参数，另一个种则少只接收一个对象

一， 多个参数

| 参数位置 | 参数类型 | 参数名称 |是否必填  | 默认值  |  其他说明  |
|:-------:|:------:|:------:|:-------:|:-------:|:-----------|
|     1   | string | 显示文本 |   是     |   -   |  如果第一个参数不是string或number类型<br>则会被当作对象来处理，也就是上面提到的另一种情况  |
|     2   | stirng | 显示图标类型 |   否     |   -   |  3种可选  'success'  ,  'error'  ,  'info'      |
|     3   | number | 关闭时间 |   否     | 1500   |  单位是毫秒ms,传其他格式（非number类型）会报错      |
|     4   | string | 文本样式类名 |   否     |   -   |  如果需要自定义显示的样式，请先定一个样式类<br>然后把类名传给该参数，定义类的时候<br>如果所有页面都使用这个类,必须定义为全局的<br>如果定义在scope作用域内的话<br>子组件不能复用父组件的样式。      |
|     5   | string | icon样式类名 |   否     |   -   |  同上，需要注意的是icon是包含在文本里面的      |
|     6   |function| 回掉函数 |   否     |   -   |  toast 隐藏时的回调hanshu      |

以下代码是一个多个参数调用的简单实例

    this.$mptoast('温馨提示', 'success', 2000)

二， 单个object对象
object对象参数的功能，其实跟上面`多个参数`的对应的功能是一样的，只是写法不同而已，我们直接看代码

    this.$mptoast({
      text: '温馨提示',        // 显示文本
      icon:'success'          // 图标类型
      duration:  2000,        // 关闭时间
      textClass: 'my-class'   // 样式类名
      iconClass: 'icon-class' // 图标类名
      callback: () => { console.log('隐藏')}
    })

需要注意的是，以上参数，如果传入错误的类型，先会进行类型转换，如果转换失败的，可能会报错。
