<script>
  export default {
    data () {
      return {
        data: null,
        width: 0,
        height: 0,
        xPos: 0,
        yPos: 0,
        delay: 1000,
        time: 0,
        timer: 0,
        waitTime: 2000,
        step: 0
      }
    },

    computed: {
      style () {
        return {
          transform: `translate(${this.xPos}px, ${this.yPos}px)`,
          transition: `all ${this.ease || 'ease-in'} ${this.delay}ms`
        }
      }
    },

    methods: {
      move () {
        cancelAnimationFrame(this.time || '')
        this.time = requestAnimationFrame(() => {
          // 这里是关健 要在动画刚好完成的时候就是达到高度的时候，不过这高度值要小于实际高度值，如实际高度值为this.height 那么这高度就要设置的比实际高度小1px也行，与实际高度一样也可以，但是不能大于实际高度，这样切换的时候就会没有痕迹
          // 最好单行的高度固定死，因为数字和汉字的高度会有差别，可能会出现滚动到切换点时高度计算的不正确会有痕迹
          if (Math.abs(this.yPos) >= this.height) {
            clearTimeout(this.timer)
            this.yPos = 0
            this.delay = 0
            this.move()
            return false
          } else {
            this.yPos -= this.step
          }
          clearTimeout(this.timer)
          this.delay = 1000
          this.timer = setTimeout(() => {
            this.move()
          }, this.waitTime)
        })
      },

      init () {
        const seamless = document.querySelector('.seamless')
        this.step = seamless.clientHeight
        const wrap = this.$refs.wrap
        this.height = wrap.clientHeight
        // TODO 优化的时候 innerHTML 只需要多加一条就行，不用把原来的HTML复制一份，量大的时候复制的也大
        wrap.innerHTML += wrap.innerHTML
        this.$nextTick(() => {
          this.move()
        })
      }
    },

    mounted () {
      this.init()
    },

    watch: {},

    beforeDestroy () {
      cancelAnimationFrame(this.time || '')
    }
  }
</script>

<template>
  <div>
    <div ref="wrap" :style="style">
      <slot></slot>
    </div>
  </div>
</template>

<style lang="scss">
</style>
