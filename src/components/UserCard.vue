<template>
  <div class="g-usercard">
    <img :src="avatar_url" alt="user avatar" class="g-usercard-avatar">
    <div class="g-usercard-content">
      <h2 class="g-usercard-name">
        <a :href="userPageUrl">
          {{name}}
        </a>
      </h2>
      <span class="g-usercard-bio">{{bio}}</span>
      <section class="g-usercard-repo">
        <a :href="userRepoUrl">{{publicRepos}}</a>
        <h4>Repos</h4>
      </section>
      <section class="g-usercard-follower">
        <a :href="userFollowerUrl">{{followers}}</a>
        <h4>Followers</h4>
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
.g-usercard {
  padding: 1rem 1rem 1.5rem 1rem;
  width: 300px;
  overflow: hidden;
  text-align: right;
  box-sizing: border-box;
  border: 1px solid;
}

.g-usercard a {
  text-decoration: none;
  transition: all 0.1s;
}

.g-usercard a:hover {
  color: #349167;
}

.g-usercard-content {
  margin-top: 1rem;
}

.g-usercard-avatar {
  height: 200px;
  width: auto;
}

.g-usercard-name {
  font-size: 2rem;
  margin: 0;
  padding: 0;
}

.g-usercard-bio {}


.g-usercard-repo {
  margin-top: 2rem;
}

.g-usercard-repo a {
  font-size: 1.5rem;
  padding: 0;
}

.g-usercard-repo h4 {
  margin: -0.3rem;
  padding: 0;
  font-weight: normal;
}

.g-usercard-follower {
  margin-top: 1.5rem;
}

.g-usercard-follower a {
  font-size: 1.5rem;
  padding: 0;
  margin: 0;
}

.g-usercard-follower h4 {
  margin: -0.3rem;
  padding: 0;
  font-weight: normal;
}
</style>
