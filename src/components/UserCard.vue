<template>
  <div class="g-usercard" :class="{lite:isLite}">
    <div class="g-usercard-avatar-container">
      <img :src="avatar_url" alt="user avatar" class="g-usercard-avatar">
    </div>
    <div class="g-usercard-content">
      <h2 class="g-usercard-name">
        <a :href="userPageUrl">
          {{name}}
        </a>
      </h2>
      <p class="g-usercard-bio">{{bio}}</p>
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
      required: true
    },
    theme: {
      type: String,
      default: 'lite'
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
    },
    isLite: function () {
      return this.theme === 'lite';
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
  /* overflow: hidden; */
  text-align: left;
  box-sizing: border-box;
  display: flex;
  border: 1px solid #526273;
  color: #2C3E50;
}

.g-usercard a {
  text-decoration: none;
  transition: all 0.1s;
  color: #42B983;
}

.g-usercard a:hover {
  color: #2C7A57;
}

.g-usercard-content {
  box-sizing: border-box;
}











/* for lite theme */

.g-usercard.lite {
  width: 400px;
  height: 200px;
}

.g-usercard.lite h4 {
  margin: 0;
  padding: 0;
}

.g-usercard.lite .g-usercard-content {
  flex: 2 2 66%;
  padding: 1rem 1rem 1rem 1.5rem;
}

.g-usercard.lite .g-usercard-avatar-container {
  flex: 1 1 33%;
}

.g-usercard.lite .g-usercard-avatar {
  width: 100%;
  height: auto;
}

.g-usercard.lite .g-usercard-name {
  font-size: 2rem;
  margin: 0;
  padding: 0;
}

.g-usercard.lite .g-usercard-bio {
  margin: 0;
  margin-bottom: 2em;
  padding: 0;
}


.g-usercard.lite .g-usercard-repo {
  width: 49%;
  display: inline-block;
  box-sizing: border-box;
}

.g-usercard.lite .g-usercard-repo a {
  font-size: 1.5rem;
  padding: 0;
}

.g-usercard.lite .g-usercard-repo h4 {
  font-weight: normal;
}

.g-usercard.lite .g-usercard-follower {
  width: 49%;
  display: inline-block;
  box-sizing: border-box;
}

.g-usercard.lite .g-usercard-follower a {
  font-size: 1.5rem;
}

.g-usercard.lite .g-usercard-follower h4 {
  font-weight: normal;
}
</style>
