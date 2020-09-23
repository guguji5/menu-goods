# Menu-Goods

### 说明

可以用与uni-app的左侧点击右侧滚动，右侧滑动左侧锚点选中的组件，具体效果见下图。
![](http://img-ys011.didistatic.com/static/dc2img/menu-goods.gif)

### Github地址
[https://github.com/guguji5/menu-goods](https://github.com/guguji5/menu-goods)

### 安装
```
npm install menugoods
```

### 引入
```
import MenuGoods from 'menugoods'
```

### 使用

```
<menu-goods 
    :left-width="150"  
    @change="handleChange" 
    :selectedGood="selectedGood" 
    :menus="menus" 
    :goods="goods" 
    :scrollHeight="scrollHeight">
</menu-goods>
```

### 属性说明

|属性名称|类型|是否必填|说明|
|----|----|----|---|
|menus|string[]|必填|左侧菜单，直接渲染成左侧菜单，也接受object[]格式,如果为object，则默认展示name属性|
|goods|object[]|必填|右侧商品，必须有menu属性，其决定着其从属于那个左侧菜单，name属性为默认展示，id用于判断是否选中|
|scrollHeight|number|必填|scroll-view组件必须传入高度才可滚动|
|leftWidth|number|选填|左侧菜单宽度，默认120px|
|selectedGood|object|必填|选中商品，应包含id，name，menu属性|
|dynamicMenusAndGoods|boolean|选填|是否需要监听menus和goods属性重新渲染数据|
|selectKey|string|选填|goods匹配选中项时的属性，默认为id|
|goodKey|string|选填|goods匹配menu菜单时的属性，默认为menu|
|menuKey|string|选填|menus为object[]时此项必填，可设置menus中与goodKey匹配的属性|
|goodTopMargin|number|选填|good的上边距，出现上下外边距合并时可设置此属性来保证滚动位置的精准|

### 事件

|事件名称|说明|返回值|
|----|----|----|
|change|点击右侧商品时触发|返回选中商品|

### 方法

|方法名称|说明|返回值|
|----|----|----|
|reset|当menus和goods变化时可手动重置该组件，默认选中第一项|无|

### SLOT

|name|说明|
|----|----|
|默认|右侧商品可用slot传入，接受child变量，其为goods子项|


```
<menu-goods :left-width="150"  @change="handleChange" :selectedGood="selectedGood" :menus="menus" :goods="goods" :scrollHeight="scrollHeight">
    <template  v-slot="{ child }">
        <view>{{ child.name }}</view>
        <view class="price">￥{{ child.price }}元</view>
    </template>
</menu-goods>
```

<details>
<summary>查看demo</summary>

```
<template>
  <view class="wrapper">
      <h1 id="component-test">西少爷肉夹馍（西二旗店）</h1>
      <menu-goods :left-width="150"  @change="handleChange" :selectedGood="selectedGood" :menus="menus" :goods="goods" :scrollHeight="scrollHeight">
        <template  v-slot="{ child }">
          <view>{{ child.name }}</view>
          <view class="price">￥{{ child.price }}元</view>
        </template>
      </menu-goods>
  </view>
</template>

<script>
import MenuGoods from '../lib/menu-goods.vue'

export default {
  components: {
    MenuGoods,
  },
  data () {
    return {
      selectedGood:{
        id:1,
        menu:'超人气套餐',
        name:'鲜辣鸡肉混沌组合',
        price:29.9
      },
      menus: ['超人气套餐','招牌原味馍','招牌酸辣粉','经典凉皮','鲜汤小混沌'],
      goods: [{
        id:1,
        menu:'超人气套餐',
        name:'鲜辣鸡肉混沌组合',
        price:29.9
      },{
        id:2,
        menu:'超人气套餐',
        name:'健康蔬菜酸辣粉套餐',
        price:34.89
      },{
        id:3,
        menu:'超人气套餐',
        name:'安心自然酸辣粉套餐',
        price:29.9
      },{
        id:4,
        menu:'超人气套餐',
        name:'招牌孜然混沌套餐',
        price:49.9
      },{
        id:5,
        menu:'招牌原味馍',
        name:'健康蔬菜夹馍',
        price:9
      },{
        id:6,
        menu:'招牌原味馍',
        name:'鲜辣鸡肉夹馍',
        price:13.5
      },{
        id:7,
        menu:'招牌原味馍',
        name:'孜然肉夹馍',
        price:12
      },{
        id:8,
        menu:'招牌原味馍',
        name:'腊汁肉夹馍',
        price:13.5
      },{
        id:9,
        menu:'招牌酸辣粉',
        name:'鸡骨浓汤酸辣粉',
        price:20
      },{
        id:10,
        menu:'招牌酸辣粉',
        name:'鸡骨浓汤酸辣粉（大份）',
        price:25
      },{
        id:11,
        menu:'经典凉皮',
        name:'金牌油泼辣子面皮',
        price:21
      },{
        id:12,
        menu:'经典凉皮',
        name:'金牌油麻将面皮',
        price:23
      },{
        id:13,
        menu:'经典凉皮',
        name:'油泼辣子面皮',
        price:23
      },{
        id:14,
        menu:'经典凉皮',
        name:'麻将面皮',
        price:23
      },{
        id:15,
        menu:'鲜汤小混沌',
        name:'麻酱传统鲜肉小混沌',
        price:24
      },{
        id:16,
        menu:'鲜汤小混沌',
        name:'麻酱荠菜鲜肉小混沌',
        price:23
      },{
        id:17,
        menu:'鲜汤小混沌',
        name:'麻酱虾仁鲜肉小混沌',
        price:24
      },{
        id:18,
        menu:'鲜汤小混沌',
        name:'传统鲜肉小混沌',
        price:20
      }],
      scrollHeight: 0
    }
  },
  watch: {

  },
  methods: {
    handleChange (good) {
      this.selectedGood = good
    }
  },
  async mounted () {
    const query = uni.createSelectorQuery().in(this)
    query.select('#component-test').boundingClientRect(res => {
      this.scrollHeight = uni.getSystemInfoSync().windowHeight - res.height 
    }).exec()
  },
}
</script>

<style lang="scss" scoped>
  #component-test{
    text-align: center;
    padding: 10px;
    font-size: x-large;
    background: white;
  }
  .price{
    color:chocolate;
    font-size: 14px;
  }
</style>
```

</details>