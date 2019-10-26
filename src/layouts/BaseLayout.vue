<template>
  <el-container style="padding-top: 0; height: 100%;" direction="vertical">
    <el-header height="72px" v-bind:class="{ 'el-header--white': scrolled }">
      <eh-header></eh-header>
    </el-header>
    <el-main style="padding-top: 0; height: 100%;" justify="space-around">
      <slot>
      </slot>
    </el-main>
  </el-container>
</template>

<script>
import Header from '@/components/Header.vue'

export default {
  /**
     * The name of the layout.
     */
  name: 'base-layout',

  data () {
    return {
      scrolled: false,
      waypoints: []
    }
  },

  computed: {},

  mounted () {
  },
  methods: {
  },
  components: {
    'eh-header': Header
  }

}
</script>
<style lang="scss" scoped>

</style>
