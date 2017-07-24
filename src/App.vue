<template>
  <div id="app">
    <h1>Demo for UserCard for GitHub</h1>
    <input type="text" v-model="username" placeholder="Please input your username">
    <button @click="update">update</button>
    <usercard :username="cardUsername"></usercard>
  </div>
</template>

<script>
import usercard from './components/UserCard.vue';
export default {
  name: 'app',
  components: {
    usercard
  },
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
      username: '',
      cardUsername: ''
    }
  },
  methods: {
    update: function () {
      this.cardUsername = this.username;
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
