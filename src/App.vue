<template>
  <div id="app">
    <News :apiKey="apiKey" />
  </div>
</template>

<script>
import News from './components/News'

export default {
  name: 'App',
  components: {
    News,
  },
  data() {
    return {
      apiKey: '7227efae15504d9db9551ff09a2c8461',
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
}
</style>
