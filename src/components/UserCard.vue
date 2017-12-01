<template>
  <div class="g-usercard" :class="theme">
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
      <section class="g-usercard-email">
        <a :href="userEmailUrl">{{email}}</a>
      </section>
      <section class="g-usercard-website">
        <a :href="website">{{website}}</a>
      </section>
      <div v-if="!theme==='lite'" class="g-usercard-flex-horizontal equal">
        <section class="g-usercard-repo">
        <h4>Repos</h4>
        <a :href="userRepoUrl">{{publicRepos}}</a>
      </section>
      <section class="g-usercard-follower">
        <h4>Followers</h4>
        <a :href="userFollowerUrl">{{followers}}</a>
      </section>
      </div>
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
      email: '',
      website: '',
      users: {}
    };
  },
  computed: {
    userPageUrl() {
      return 'https://github.com/' + this.username;
    },
    userRepoUrl() {
      return `https://github.com/${this.username}?tab=repositories`;
    },
    userFollowerUrl() {
      return `https://github.com/${this.username}?tab=followers`;
    },
    userEmailUrl() {
      return `mailto:${this.email}`;
    }
  },
  watch: {
    username: function(newUsername) {
      this.fetchUserInfo(newUsername);
    }
  },
  methods: {
    fetchUserInfo(username) {
      if (!username) return;
      console.log('fetching ' + username);
      if (this.users[username]) {
        let data = this.users[username];
        this.name = data.name;
        this.avatar_url = data.avatar_url;
        this.bio = data.bio;
        this.publicRepos = data.public_repos;
        this.followers = data.followers;
        this.email = data.email;
        this.website = data.blog;
      } else {
        axios
          .get(`https://api.github.com/users/${username}`)
          // user arrow function to solve the problem with this
          .then(response => {
            let data = response.data;
            console.log(data);
            this.users[username] = data;
            this.name = data.name;
            this.avatar_url = data.avatar_url;
            this.bio = data.bio;
            this.publicRepos = data.public_repos;
            this.followers = data.followers;
            this.email = data.email;
            this.website = data.blog;
          })
          .catch(error => {
            console.log(error);
            this.name = 'Error! User Not Found';
          });
      }
    }
  }
};
</script>
<style>
.g-usercard {
  /* overflow: hidden; */
  text-align: left;
  box-sizing: border-box;
  display: flex;
  color: #2c3e50;
}

.g-usercard a {
  text-decoration: none;
  transition: all 0.1s;
  color: #42b983;
}

.g-usercard a:hover {
  color: #2c7a57;
}

.g-usercard-content {
  box-sizing: border-box;
}

.g-usercard-flex-horizontal {
  display: flex;
}

/* for lite theme */

.g-usercard.lite {
  width: 229px;
  height: 400px;
  border-radius: 2px;
  overflow: hidden;
  flex-direction: column;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
}

.g-usercard.lite h4 {
  margin: 0;
  padding: 0;
}

.g-usercard.lite .g-usercard-content {
  flex: 2 2 66%;
  padding: 1rem;
}

.g-usercard.lite .g-usercard-avatar-container {
  flex: 1 1 33%;
}

.g-usercard.lite .g-usercard-avatar {
  width: 100%;
  height: auto;
}

.g-usercard.lite .g-usercard-name {
  font-size: 1.5rem;
  margin: 0;
  padding: 0;
}

.g-usercard.lite .g-usercard-bio {
  margin: 0;
  margin-bottom: 1.5rem;
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
