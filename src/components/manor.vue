<template>
  <div>
    <div class="manor-wrapper">
      <div class="manor-header">
        <div class="logo-wrapper">
          <img class="logo" src="../assets/logo.png" alt="">
        </div>
        <div class="manor-name" @click="jump('manor')">{{kingdom}}</div>
      </div>
      <div class="manor-main-wrapper">
        <div class="info-wrapper">
          <div class="row">王国：{{kingdom}}</div>
          <div class="row">领主：{{nickname}}</div>
          <div class="row">坐标：({{capitalX}}, {{capitalY}})</div>
        </div>
        <div class="links-wrapper">
          <button class="link" @click="jump('building')">建筑</button>
          <button class="link" @click="jump('plat')">地图</button>
          <button class="link" @click="jump('#')">军事</button>
        </div>
      </div>
    </div>
    <VFooter />
  </div>
</template>

<script>
  import VResourceBar from './sub/resource-bar'
  import VFooter from './sub/v-footer'
  import VBuildingList from './sub/building-list'
  import VProgress from './sub/progress'
  export default {
    name: 'manor',
    data () {
      return {
        activeType: 'farm',
        actionNumber: 23,
      }
    },
    components: { VResourceBar, VFooter, VProgress, VBuildingList },
    methods: {
      toggle: function () {
        this.axios.post('login', this.loginForm).then((response) => {
          console.info(response.data)
          this.$store.commit('setUser', response.data.user)
          localStorage.setItem('user', JSON.stringify(response.data.user))
          localStorage.setItem('resource', JSON.stringify(response.data.resource))
          localStorage.setItem('building', JSON.stringify(response.data.building))
          window.location = '/#/manor'
        }).catch((error) => {
          this.$swal({
            text: (error.response.data) ? error.response.data : '服务器出错',
            type: 'error',
          })
        })
      },
    },
    computed: {
      nickname () {
        return this.$store.state.nickname
      },
      kingdom () {
        return this.$store.state.kingdom
      },
      capitalX () {
        return this.$store.state.capitalX
      },
      capitalY () {
        return this.$store.state.capitalY
      },
      resource () {
        return this.$store.state.resource
      },
    },
    created: function () {
      // 检查用户登录状态
      console.info(this.checkLogin())
      if (!this.checkLogin()) {
        window.location = '/#/'
      }

      // 赋值数据
      let user = JSON.parse(localStorage.getItem('user'))
      this.$store.commit('setUser', user)
    },
  }
</script>

<style scoped lang="stylus">
  .manor-wrapper
    width: 100%
    min-height: 87.88vh
    .manor-header
      padding: 2rem 3rem
      text-align: left
      /*display: flex
      align-items: center
      justify-content: space-between*/
      .logo-wrapper
        display: inline-block
        width: 40%
        height: 60px
        line-height: 60px
        text-align: left
        vertical-align: middle
        .logo
          width: 300px
          height: 60px
      .manor-name
        margin 0 auto
        width: 40%
        height: 60px
        line-height: 60px
        font-size: 1.8rem
        text-align: center
        vertical-align: middle
    .manor-main-wrapper
      width: 100%
      .info-wrapper
        margin: 0 auto
        padding: 2rem
        width: 440px
        /*height: 360px*/
        -webkit-box-shadow:  0 2px 8px #cccccc
        -moz-box-shadow: 0 2px 8px #cccccc
        box-shadow: 0 2px 8px #cccccc
        -webkit-border-radius: 4px
        -moz-border-radius: 4px
        border-radius: 4px
        .row
          height: 3rem
          line-height: 3rem
          text-align: left
          font-size: 1.2rem
      .links-wrapper
        width: 100%
        text-align: center
        .link
          margin: 3rem 2rem
</style>

<style scoped lang="scss">
  @import "../assets/scss/global_css";
</style>
