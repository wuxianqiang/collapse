<template>
  <div>
    <div class="ball-container">
      <div v-for="(item, index) in balls" :key="index">
        <transition
          @before-enter="beforeEnter"
          @enter="enter"
          @after-enter="afterEnter">
          <div class="boll" v-if="item.show">
            <div class="inner inner-hook"></div>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      balls: [
        {show: false},
        {show: false},
        {show: false},
        {show: false},
        {show: false}
      ],
      dropBalls: []
    }
  },
  methods: {
    drop (el) {
      console.log(el)
      for (let i = 0; i< this.balls.length; i++){
        const ball = this.balls[i]
        if (!ball.show) {
          ball.show = true,
          ball.el = el
          this.dropBalls.push(ball)
          return
        }
      }
    },
    beforeEnter (el) {
      const ball = this.dropBalls[this.dropBalls.length - 1]
      const rect = ball.el.getBoundingClientRect()
      const x = rect.left - 32
      const y = -(window.innerHeight - rect.top - 22)
      el.style.display = 'block'
      el.style.transform = `translate3d(0,${y}px,0)`
      const inner = el.getElementsByClassName('inner-hook')[0]
      inner.style.transform = `translate3d(${x}px,0,0)`
    },
    enter (el, done) {
      this._reflow = document.body.offsetHeight || document.documentElement.offsetHeight
      el.style.transform = `translate3d(0,0,0)`
      const inner = el.getElementsByClassName('inner-hook')[0]
      inner.style.transform = `translate3d(0,0,0)`
      el.addEventListener('transitionend', done)
    },
    afterEnter (el) {
      const ball = this.dropBalls.shift()
      if (ball) {
        ball.show = false
        el.style.display = 'none'
      }
    }
  }
}
</script>

<style scoped>
  .boll {
    /* 外层 */
    position: fixed;
    left: 32px;
    bottom: 22px;
    transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
  }
  /* 里层 */
  .inner {
    width: 16px;
    height: 15px;
    border-radius: 50%;
    background: blue;
     transition: all 0.4s linear
  }
</style>
