<template>
  <div id="app">
    <h1>Demo for GitHub Usercard</h1>
    <div class="app-console">
      <p>Plase input your username bellow to see how it works</p>
      <input type="text" v-model="username" placeholder="Please input your username">
      <button @click="update">update</button>
    </div>
    <div class="app-component-display">
  
      <usercard :username="cardUsername"></usercard>
  
    </div>
  
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
      username: 'tianhanl',
      cardUsername: 'tianhanl'
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

.app-console {
  margin: 2rem;
}

.app-component-display {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
