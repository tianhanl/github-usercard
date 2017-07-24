<template>
  <div class="g-usercard">
    <img :src="avatar_url" alt="user avatar" class="g-usercard-avatar">
    <div class="g-usercard-content">
      <h2 class="g-usercard-name">
        <a :href="userPageUrl">
          {{name}}
        </a>
      </h2>
      <span class="g-usercard-bio"></span>
      <section class="g-usercard-repo">
        <h3>Repos:</h3>
        <a :href="userRepoUrl">{{publicRepos}}</a>
      </section>
      <section class="g-usercard-follower">
        <h3>Followers</h3>
        <a :href="userFollowerUrl">{{followers}}</a>
      </section>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  name: 'usercard',
  props: {
    username: {
      type: String,
      required: true,
      default: 'tianhanl'
    }
  },
  created() {
    this.fetchUserInfo(this.username);
  },
  data() {
    return {
      name: 'N/A',
      avatar_url: '',
      bio: '',
      publicRepos: 0,
      followers: 0,
      users: {}
    }
  },
  computed: {
    userPageUrl: function () {
      return 'https://github.com/' + this.username;
    },
    userRepoUrl: function () {
      return `https://github.com/${this.username}?tab=repositories`
    },
    userFollowerUrl: function () {
      return `https://github.com/${this.username}?tab=followers`
    }
  },
  watch: {
    username: function (newUsername) {
      this.fetchUserInfo(newUsername);
    }
  },
  methods: {
    fetchUserInfo(username) {
      if (!username) return;
      console.log('fetching ' + username)
      if (this.users[username]) {
        let data = this.users[username];
        this.name = data.name;
        this.avatar_url = data.avatar_url;
        this.bio = data.bio;
        this.publicRepos = data.public_repos;
        this.followers = data.followers;
      } else {
        axios
          .get('https://api.github.com/users/' + username)
          // user arrow function to solve the problem with this
          .then(response => {
            let data = response.data;
            this.users[username] = data;
            this.name = data.name;
            this.avatar_url = data.avatar_url;
            this.bio = data.bio;
            this.publicRepos = data.public_repos;
            this.followers = data.followers;
          })
          .catch(error => {
            console.log(error);
            this.name = 'Error! User Not Found'
          })
      }
    }
  }



}

</script>
<style>

</style>
