<script>
  import seamless from '@/components/seamless'
  export default {
    data () {
      return {
        data: null
      }
    },

    components: {
      seamless
    },

    methods: {
      initEnter () {
        console.log('initEnter')
      }
    },

    mounted () {
      console.log('mounted')
    }
  }
</script>

<template>
  <div class='content'>
    <seamless class="seamless" :listNum="3">
      <!-- <p>我我我我我我</p>
      <p>你你你你你你</p> -->
      <p>11</p>
      <p>22</p>
    </seamless>
  </div>
</template>

<style lang="scss" scoped>
  .seamless {
    height: 60px;
    line-height: 60px;
    overflow: hidden;
    background: red;
  }
  .seamless-item {
    height: 60px;
    background: green;
  }
</style>
