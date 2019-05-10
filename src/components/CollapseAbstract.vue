<script>
export default {
  // 在 Vue 中启用抽象组件
  abstract: true,
  props: {
    title: {
      type: String,
      default: '操作'
    }
  },
  data () {
    return {
      height: 0,
      isShow: false
    }
  },
  // 我们不需要任何包裹的元素，只需要返回里面的内容即可
  render: function (createElement) {
    if (this.$slots.default) {
      this.height = this.$slots.default[0]
    } else {
      this.height = 0
    }
    return createElement(
      'div',   // 标签名称
      [
        createElement(
          'div',
          {
            on: {
              click: this.clickHandler
            },
          },
          [this.title]
        ),
        createElement(
          'div',
          {
            style: {
              height: 0,
              transition: '0.3s height ease-in-out',
              overflow: 'hidden'
            },
            ref: 'myRef',
          },
          this.$slots.default
        )
      ]
    )
  },
  methods: {
    clickHandler () {
      this.isShow = !this.isShow
      if (this.isShow) {
        let height = this.height.elm.offsetHeight
        this.$refs.myRef.style.height = `${height}px`
        this.$refs.myRef.addEventListener('transitionend', () => {
          this.$emit('openEnd')
        })
      } else {
        this.$refs.myRef.style.height = 0
        this.$refs.myRef.addEventListener('transitionend', () => {
          this.$emit('closeEnd')
        })
      }
    }
  }
}
</script>
