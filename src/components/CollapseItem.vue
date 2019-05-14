<template>
  <div>
    <div @click="handleClick" >
      <slot name="title">
        标题
      </slot>
    </div>
    <transition
      @before-enter="beforeEnter"
      @enter="enter"
      @after-enter="afterEnter"
      @leave="leave"
      @after-leave="afterLeave"
    >
      <div  v-show="isShow" class="content">
        <slot></slot>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  data () {
    return {
      isShow: false
    }
  },
  methods: {
    handleClick () {
      this.$parent.changeState(this._uid)
    },
    beforeEnter (el) {
      // 动画之前
      el.style.height = 0
    },
    enter (el, done) {
      // 动画过程
      let height = el.scrollHeight
      let step = height / 300 * 20
      let distance = 0
      let change = () => {
        distance += step
        if (distance < height) {
          el.style.height = `${distance}px`
          requestAnimationFrame(change)
        } else {
          el.style.height = `${height}px`
          done()
        }
      }
      requestAnimationFrame(change)
    },
    afterEnter (el) {
      // 动画之后
      console.log('展开动画完成')
    },
    leave (el, done) {
      // 动画过程
      let height = el.scrollHeight
      let step = height / 300 * 20
      let change = () => {
        height -= step
        if (height > 0) {
          el.style.height = `${height}px`
          requestAnimationFrame(change)
        } else {
          el.style.height = 0
          done()
        }
      }
      requestAnimationFrame(change)
    },
    afterLeave (el) {
      console.log('关闭动画完成')
    }
  }
}
</script>

<style scoped>
.content {
  overflow: hidden;
}
</style>

