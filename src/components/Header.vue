<template>
  <div
    id="navbar"
    class="el-row is-justify-space-between is-align-middle animated el-row--flex animated--visible fadeInUp offsetted"
  >
    <div class="container-logo">
      <router-link :to="{name: 'communities'}">
        <img src="../assets/images/logo.svg" alt="img_logo" />
      </router-link>
    </div>
    <div class="container_menu">
      <img src="../assets/images/Hamburguer_background.svg" alt="img_menu" class="menu_img" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Header',
  data () {
    return {}
  },
  mounted () {},
  methods: {},
  components: {}
}
</script>

<style lang="scss" scoped>
@import "~/assets/scss/element-variables.scss";
@import "~/assets/scss/styles.scss";

.container_menu {
  position: relative;
}
.menu_img {
  position: absolute;
  top: -42px;
  right: -28px;
}
</style>
