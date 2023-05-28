<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->


  <LoginVue outputText = 'sdfg'>  </LoginVue>
  <ChatVue >  </ChatVue>

</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import LoginVue from './components/LoginVue.vue'
import ChatVue from './components/ChatVue.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld,
    LoginVue,
    ChatVue
  }
}

window.addEventListener('unload', function() {
   localStorage.removeItem('token');
});
 
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
