<template>
  <div class="ratingselect">
    <div class="rating-type">
      <span @click="select(2, $event)" :class="{'active':selectType===2}" class="block positive">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
      <span @click="select(0, $event)" :class="{'active':selectType===0}" class="block positive">{{desc.positive}}<span class="count">{{positives.length}}</span></span>
      <span @click="select(1, $event)" :class="{'active':selectType===1}" class="block negative">{{desc.negative}}<span class="count">{{negatives.length}}</span></span>
    </div>
    <div @click="toggleContent" class="switch" :class="{'on': onlyContent}">
      <span class="icon-check_circle"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
const ALL = 2
const POSITIVE = 0
const NEGATIVE = 1
export default {
  props: {
    ratings: {
      type: Array,
      default() {
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
      default() {
        return {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      }
    }
  },
  methods: {
    select(type, event) {
      if (!event._constructed) {
        return 
      }
      this.$emit('select', type)
    },
    toggleContent(event) {
      if (!event._constructed) {
        return 
      }
      this.$emit('toggle')
    } 
  },
  computed: {
    positives() {
      return this.ratings.filter((rating) => {
        return rating.rateType === POSITIVE
      })
    },
    negatives() {
      return this.ratings.filter((rating) => {
        return rating.rateType === NEGATIVE
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import "../../common/stylus/mixin.styl"
  .ratingselect
    .rating-type
      margin: 0 18px
      padding: 18px 0
      border-1px(rgba(7,17,27,0.1))
      font-size: 0
      .block
        display: inline-block
        padding: 8px 12px
        margin-right: 8px
        border-radius: 2px
        font-size: 12px
        color: rgb(77,85,93)
        &.active
          color: #fff
        .count
          margin-left: 2px
          line-height: 16px
          font-size: 8px
        &.positive
          background: rgba(0,160,220,0.2)
          &.active
            background: rgb(0,160,220)
        &.negative
          background: rgba(77,85,93,0.2)
          &.active
            background: rgb(77,85,93)
    .switch
      padding: 12px 18px
      border-bottom: 1px solid rgba(7,17,27,0.1)
      line-height: 24px
      color: rgb(147,153,159)
      font-size: 0
      &.on
        .icon-check_circle
          color: #00c850
      .icon-check_circle
        display: inline-block
        margin-right: 4px
        vertical-align: top
        font-size: 24px
      .text
        display: inline-block
        font-size: 12px
</style>