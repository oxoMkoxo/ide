<template>
  <div id="app">
    <router-view></router-view>
    <notifications/>
  </div>
</template>

<script>
  import Editor from './components/Editor'
  import { setToken } from './utils/api'

  export default {
    name: 'app',
    components: {
      Editor
    },
    created() {
      if (this.$store.state.user.isAuthenticated) {
        setToken(this.$store.state.user.token)
      }
      if (this.$route.name != 'saved') {
        this.$store.commit('resetCode')
      }
    },
    mounted() {
      let mutationsToSubscribe = [
        'changeTheme',
        'changeFont',
        'changeFontSize',
        'resetEditor',
      ]

      this.$store.subscribe((mutation, state) => {
        if (mutationsToSubscribe.includes(mutation.type) && !state.isChanged) {
          this.$store.commit('setIsChanged', true)
        }
      })
      const wrapperHeightObserver = new ResizeObserver(entries => {
        var heightOfBanner = (entries[0].contentRect.height - 45)+"px"; //calculate height of promotion-banner
        // var ekaur = (entries[0].contentRect.height)+"px";
        document.getElementById('editor').setAttribute('style', `height: calc(100vh - 75px - ${heightOfBanner})`);
        var inoutElem = document.querySelector('#inoutbox');
        inoutElem.setAttribute('style', `--topRelativeToBanner: ${heightOfBanner}`);
      });

      const wrapperDiv = document.querySelector('.wrapper');
      wrapperHeightObserver.observe(wrapperDiv);
    }
  }

</script>
<style src="bootstrap/dist/css/bootstrap.css"></style>
<style src="font-awesome/css/font-awesome.css"></style>
<style src="./assets/css/whole.css"></style>
<style scoped>
  #app {
    background: #202020;
  }
</style>
