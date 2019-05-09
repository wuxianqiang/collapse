<template>
  <div>
    <slot></slot>
  </div>
</template>

<script>
export default {
  methods: {
    changeState (id) {
      this.$children.forEach(child => {
        if (child._uid !== id) {
          child.isShow = false
        } else {
          child.isShow = !child.isShow
        }
      })
    }
  }
}
</script>
