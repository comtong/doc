## 目录
## 目录
* [1. Vue 工具使用](#Vue学习笔记)
  * [1.1 Vue模板创建](#Vue模板创建)
* [2. Vue小技巧](#Vue小技巧)
* [3. 前端开发常见bug](#前端开发常见bug)
* [3. 自测检查单](#自测检查单)

## 1. Vue学习笔记
### 1.1 vue模板创建
  安装vetur ——> 菜单栏-code-首选项-用户代码片段 ——> 编辑vue.json

## 2 Vue小技巧
  （1）利用@hook:updated="doSomething" @hook:+生命周期=‘回调函数’, 可以获取第三方组件的生命周期的钩子回调 
  （2）css引用变量
<script>
computed: {
    style () {
      let styles = {
        'flex-grow': this.flexGrow,
        'flex-shrink': this.flexShrink,
        '--margin': `${this.$parent.gutter}px`  // 动态设置变量
      }
      return styles
    },
    classs () {
      if (this.$parent.direction === 'column') {
        return 'tui-flex-item_column'
      } else {
        return 'tui-flex-item_row'
      }
    }
}
</script>
<style lang="less" scoped>
  .tui-flex-item_row:nth-child(n+2) {
    margin-left: var(--margin)
  }
  .tui-flex-item_column:nth-child(n+2) {
    margin-top: var(--margin)
  }
</style>
 （3）Vue中require(path)需要注意：
   webpack本身是一个预编译打包工具，无法预测未知变量路径，不能require纯变量路径，然后path至少由三部分组成，目录+文件名+后缀
   目录 => webpack才知道从哪里查找
   后缀 => 文件后缀，必须加上，不然会报错
   文件名 => 可用变量表示
   例子： require(`../../assets${this.imgSrc}.png`)

 （4）vue cli3.0 build 打包 的 js 文件添加版本号 解决 js 缓存问题
     在 vue.config.js 的文件中加入下面这段话

    // vue.config.js
    const Timestamp = new Date().getTime();
    module.exports = {
      configureWebpack: { // webpack 配置
        output: { // 输出重构  打包编译后的 文件名称  【模块名称.版本号.时间戳】
          filename: `[name].${process.env.VUE_APP_Version}.${Timestamp}.js`,
          chunkFilename: `[name].${process.env.VUE_APP_Version}.${Timestamp}.js`
        },
      },
    ...
    }

## 3 前端开发常见bug
(1) bug: 9.99 + 4.00 * 2 = 17.000000023
    解决：加toFixed(2),但会引发新问题，因为toFixed(2)会转为String，和别的数字相加会造成 5.00 + '100.00' = 5.00100.00
    所以还得加上 pasreFloat(5.00) + parseFloat(100.00)
(2) flex布局的时候，要注意超长判断，特别是用了span，一个超长会挤压其他的
(3) bug：文字如果是纯数字或者纯英文的超长单词，，因为默认超长单词不会换行，所以样式就会挤出去
    解决：word-break: break-all 强制断开单词
(4) bug：数组在删除数据的时候，如果删除第一个元素，数据是索引就变了，那么如果还是按照顺序去删除第二个，那么就会漏掉一个，甚至会数组越界
    解决：先排序，再逆向删除
(5) bug：dialog关闭后再重新打开，如果没有重置数据，那么会自动填充原来的数据
    解决：关闭后或者每次新打开都要重置数据
(6) bug：搜索条件变化后重新搜索如果不重置页码，会导致搜索结果不正常，因为页码如果此时不是1，那么一旦新的搜索条件下，没有第二页数据，就会返回空
    解决：每次搜索条件变化后必须重置页码数据
(7) bug：div和img之前会有间隙
    原因：div中存在img标签，由于img标签的display:inline-block属性。
         display:inline-block布局元素在chrome下会出现几像素的间隙，原因是因为我们在编辑器里写代码的时候，同级别的标签不写在同一行
    解决：修改display

(8) bug：input一旦type=number，maxlength就用不了（IE可以，Chrome不可以）
    解决：想要达到此效果，需要在input事件中添加

    onInput(value, att) {
          if (value.length > 8) {
          this.$nextTick(() => {
            this.value[att] = value.slice(0, 8)
          })
        }
      }
(9) bug：如果用ajax，默认是form表单形式提交的
    解决：需要手动设置 contentType: 'application/json;charset:utf-8',  才会用正常的post请求

## 4 自测检查单
（1）任何字符串都要考虑超长和换行的问题，特别是纯数字纯英文这种
（2）文案要对照清楚，非常容易出Bug
（3）各种为空的情况要注意，要特殊处理
（4）输入框的规范和长度限制要提前和产品沟通好