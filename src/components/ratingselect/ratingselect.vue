<template>
  <div class="ratingselect">
    <div class="rating-type border-1px">
      <span class="block positive" @click="select(2,$event)" :class="{'active':selectType===2}">{{desc.all}}<span
        class="count">{{ratings.length}}</span></span>
      <span class="block positive" @click="select(0,$event)" :class="{'active':selectType===0}">{{desc.positive}}<span
        class="count">{{positives.length}}</span></span>
      <span class="block negative" @click="select(1,$event)" :class="{'active':selectType===1}">{{desc.negative}}<span
        class="count">{{negatives.length}}</span></span>
    </div>
    <div @click="toggleContent($event)" class="switch" :class="{'on':onlyContent}">
      <i class="icon-check_circle"></i>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  /*正面评论*/
  const POSITIVE = 0;
  /*负面评论*/
  const NEGATIVE = 1;
  /*所有评论*/
  const ALL = 2;

  export default {
    props: {
      ratings: {
        type: Array,
        default(){
          return []
        }
      },
      selectType: {
        type: Number,
        default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: false
      },
      desc: {
        type: Object,
        default(){
          return {
            all: '全部',
            positive: '满意',
            negative: '不满意'
          }
        }
      }
    },
    methods: {
      /*评价按钮点击切换*/
      select(type, event){
        /*禁止原生事件的响应主要原因是有滚动条的那个插件*/
        if (!event._constructed) {
          return
        }
        /*子组件告诉父组件数据变化*/
        this.$emit('select', type);
      },
      toggleContent(event){
        /*禁止原生事件的响应主要原因是有滚动条的那个插件*/
        if (!event._constructed) {
          return
        }
        /*子组件告诉父组件数据变化*/
        this.$emit('toggle', this.onlyContent);
      }
    },
    computed: {
      positives(){
        return this.ratings.filter((rating) => {
          return rating.rateType === POSITIVE;
        })
      },
      negatives(){
        return this.ratings.filter((rating) => {
          return rating.rateType === NEGATIVE;
        })
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .ratingselect
    .rating-type
      padding 18px 0
      margin 0 18px
      border-1px(rgba(7, 17, 27, 0.1))
      font-size 0
      .block
        display inline-block
        line-height 16px
        padding 8px 12px
        margin-right 8px
        border-radius 1px
        color rgb(77, 85, 93)
        font-size 12px
        &.active
          color #fff
        &.positive
          background rgba(0, 160, 220, 0.2)
          &.active
            background rgb(0, 160, 220)
        &.negative
          background rgba(77, 85, 93, 0.2)
          &.active
            background rgb(77, 85, 93)
        .count
          line-height 16px
          margin-left 2px
          font-size 8px
    .switch
      padding 12px 18px
      line-height 24px
      border-bottom 1px solid rgba(7, 17, 27, 0.1)
      color rgb(147, 153, 159)
      font-size 0
      &.on
        .icon-check_circle
          color #00c850
      .icon-check_circle
        display inline-block
        margin-right 4px
        font-size 24px
        vertical-align top
      .text
        display inline-block
        vertical-align top
        font-size 12px
</style>
