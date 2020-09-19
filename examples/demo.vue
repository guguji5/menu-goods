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
