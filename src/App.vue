<template>
  <div id="app">
    <ul>
      <li v-for="user in users" v-bind:key="user.id">
        <img v-bind:src="user.avatar_url" />
        {{ user.login }}
      </li>
    </ul>
    <button @click="refresh">Refresh</button>
    <button @click="loadMore">Load More</button>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import axios from "axios";
const baseurl = "https://api.github.com/";
const pathname = "users";

export default {
  name: "App",
  data: function() {
    return {
      users: []
    };
  },
  mounted() {
    axios.get(baseurl + pathname).then(data => {
      this.users = data.data;
      console.log(data.data);
    });
  },
  methods: {
    refresh() {
      axios
        .get(
          `${baseurl}${pathname}?since=${this.users[this.users.length - 1].id}`
        )
        .then(response => {
          this.users = response.data;
          console.log(this.users);
        });
    },
    loadMore() {
      axios
        .get(
          `${baseurl}${pathname}?since=${this.users[this.users.length - 1].id}`
        )
        .then(response => {
          this.users = [...this.users, ...response.data];
          console.log(this.users);
        });
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
