<template>
    <view class ="menu-goods" :id="'anchor'+anchorId">
        <view class="left" :style="'min-width: '+leftWidth+'px'">
            <view
              v-for="(item,index) in menus" :key="index" :class="{'active': list[index].active}"
              @click="changeTab(item,index)">
              {{renderMenuValue(item)}}
            </view>
          </view>
          <scroll-view
            class="scroll-Y"
            :scroll-y="true"
            :style="height"
            :scroll-top="scrollTop"
            scroll-with-animation="true"
            @scroll="pageScroll"
          >
          <view class="right">
            <view v-for="(item, index) in displayGoods" :key="index">
                <view :id="'s'+index">
                  <view
                    class="panel"
                    v-for="(child, i) in item"
                    :class="{'active':selectedGood[selectKey] === child[selectKey]}"
                    @click="handleActivePanel(child, i)"
                    :key="i">
                    <slot v-bind:child="child">
                        {{ child.name }}
                    </slot>
                  </view>
                </view>
            </view>
            <view :style="'height:'+lastDataAppendHeight+'px;border; 1px solid red'"></view>
          </view>
        </scroll-view>
      </view>
</template>

<script>
export default {
  data () {
    return {
      list: [],
      activeID: this.activityId,
      isAsyncRefresh: false,
      scrollTop: 0,
      old: {
        scrollTop: 0
      }
    }
  },
  props: {
    menus: {
      type: Array,
      default: []
    },
    menuKey: {
      type: String
    },
    goods: {
      type: Array,
      default: []
    },
    scrollHeight: {
      type: Number
    },
    leftWidth: {
      type: Number
    },
    selectedGood: {
      type: Object
    },
    dynamicMenusAndGoods: {
      type: Boolean,
      default: false
    },
    selectKey: {
      type: String,
      default: 'id'
    },
    goodKey: {
      type: String | Array,
      default: 'menu'
    },
    goodTopMargin:{
      type: Number,
      default: 0
    }
  },
  computed: {
    height () {
      return `height: ${this.scrollHeight}px`
    },
    // 如果最后一项元素过少，锚点定位会滚不上去，需要补齐透明元素
    lastDataAppendHeight () {
      if (this.list.length === 0) return 0
      return Math.max(this.scrollHeight - this.list[this.list.length - 1].height, 0)
    },
    displayGoods () {
      const result = this.menus.map(_ => [])
      for (const item of this.goods) {
        for (let i = 0; i < this.menus.length; i++) {
          if (this.getGoodValue(item) === this.getMenuValue(this.menus[i])) {
            result[i].push(item)
          }
        }
      }
      return result
    }
  },
  watch: {
    menus: {
      handler (nv) {
        if (nv && this.dynamicMenusAndGoods) {
          this.init()
        }
      },
      deep: true
    },
    goods: {
      handler (nv) {
        if (nv && this.dynamicMenusAndGoods) {
          this.init()
        }
      },
      deep: true
    }
  },
  methods: {
    init () {
      let selectActive = false
      this.list = this.menus.map(() => {
        return {
          active: false,
          height: 0
        }
      })

      for (let i = 0; i < this.menus.length; i++) {
        if (this.getGoodValue(this.selectedGood) === this.getMenuValue(this.menus[i])) {
          selectActive=true
          this.list[i].active = true
        }
      }
      if(!selectActive){
        this.list[0].active=true
      }
      setTimeout(() => {
        this.menus.forEach((item, key) => {
          uni.createSelectorQuery().in(this).select(`#s${key}`).boundingClientRect(res => {
            this.list[key].height = res.height
          }).exec()
        })
      }, 100)
    },
    handleActivePanel (child, i) {
      this.$emit('change', child, i)
    },
    pageScroll (e) {
      this.old.scrollTop = e.detail.scrollTop
      let index = 0
      let position = this.list[0].height

      if (!this.isAsyncRefresh) {
        this.isAsyncRefresh = true
        setTimeout(() => {
          let index = 0
          let position = this.list[0].height
          uni.createSelectorQuery().in(this).select('.scroll-Y').scrollOffset(res => {
            while (position <= res.scrollTop) {
              index++
              position += this.list[index].height
            }
            this.list.forEach((key, i) => {
              key.active = i === index
            })
          }).exec()
          this.isAsyncRefresh = false
        }, 100)
      }

      while (position <= e.detail.scrollTop) {
        index++
        position += this.list[index].height
      }
      this.list.forEach((key, i) => {
        key.active = i === index
      })
    },
    reset () {
      this.list = this.menus.map((item, i) => {
        return {
          active: i === 0,
          height: 0
        }
      })
      this.scrollTop = this.old.scrollTop
      this.$nextTick(function () {
        this.scrollTop = 0
      })
      setTimeout(() => {
        this.menus.forEach((item, key) => {
          uni.createSelectorQuery().in(this).select(`#s${key}`).boundingClientRect(res => {
            this.list[key].height = res.height
          }).exec()
        })
      }, 100)
    },
    changeTab (val, i) {
      let index = 0
      let position = 0
      while (index < i) {
        position += this.list[index].height + this.goodTopMargin
        index++
      }
      this.scrollTop = this.old.scrollTop
      this.$nextTick(function () {
        this.scrollTop = position
      })

      this.list.forEach(item => {
        item.active = false
      })
      this.list[i].active = true
    },
    renderMenuValue(item){
      return typeof(item)==='object' ? item.name : item
    },
    getMenuValue (item) {
      if (typeof (item) === 'object' && this.menuKey) {
        return item[this.menuKey]
      }
      return item
    },
    getGoodValue (item) {
      const goodKey = [].concat(this.goodKey)

      if (typeof (goodKey) === 'object' && Array.isArray(goodKey) && goodKey.length > 0) {
        let currKey = goodKey.shift()
        let result = item[currKey]
        while (goodKey.length > 0) {
          currKey = goodKey.shift()
          result = result[currKey]
        }
        return result
      }
      return item[goodKey]
    }
  },
  mounted () {
    this.init()
  }
}
</script>

<style lang="scss" scoped>
@import "style/font.scss";
.menu-goods {
  background: #F2F6FC;
  display: flex;
  .left{
      background: #fff;
    view{
        height: 20px;
        line-height: 20px;
        padding: 16px 0 16px 16px;

    }
    .active {
        color: #2F81F9;
        border-right: 2px solid #2F81F9;
    }
  }
  .right{
    .panel{
        &.active{
            background: rgba(47, 129,249,0.12);
            border-color: #2F81F9;
        }
        margin: 8px 16px 0px;
        padding: 10px 12px 8px;
        border: 1px solid #EAEEF5;
        border-radius: 2px;
        background: white;
        .title{
            font-family: PingFangSC-Medium;
            .titleText{
                color: #101724;
                margin-right: 4px;
                font-size: 16px;
                line-height: 22px;
            }
            .discount{
                color: #fff;
                font-size: 12px;
                line-height: 14px;
                padding: 1px 4px;
                background: #EF645C;
            }
            .price{
                color: EF645C;
                float: right;
                font-size: 16px;
                line-height: 22px;
            }
        }
        .subtitle{
            margin-top: 4px;
            color: #505568;
        }
        .divider{
            margin-top: 8px;
            height: 8px;
            border-top: 1px solid #D8D8D8;
        }
        .activity{
            color: #EF645C;
            text{
                line-height: 20px;
                margin-right: 8px;
            }
        }
    }
  }
}
</style>
