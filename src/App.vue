<template>
  <div id="app" class="mx-auto mt-10 pt-10">
    <form class="w-full max-w-sm" @submit.prevent="submit" v-if="!loading">
      <div class="flex items-center border-b border-b-2 border-teal-500 py-2">
        <input
          class="appearance-none bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none"
          type="text"
          placeholder="Username"
          aria-label="Username"
          v-model="username"
        />
        <button
          class="flex-shrink-0 bg-teal-500 hover:bg-teal-700 border-teal-500 hover:border-teal-700 text-sm border-4 text-white py-1 px-2 rounded"
          type="submit"
        >Sign Up</button>
      </div>
    </form>
    <div v-if="loading" class="text-white">Loading...</div>
    <div id="user-board" v-if="result">Board Result</div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "app",
  data() {
    return {
      username: "",
      loading: false,
      result: false
    };
  },
  methods: {
    submit() {
      this.loading = true;
      axios
        .get(
          `https://fortnite-api.theapinetwork.com/users/id?username=${this.username}`,
          {
            headers: {
              Authorization: "abfcf665833c8774ff5163a564be343d"
            }
          }
        )
        .then(response => {
          let uid = response.data.data.uid;
          console.log(`Get the uid: ${uid}`);
          axios
            .get(
              `https://fortnite-api.theapinetwork.com/prod09/users/public/br_stats_v2?user_id=${uid}`,
              {
                headers: {
                  Authorization: "abfcf665833c8774ff5163a564be343d"
                }
              }
            )
            .then(response => {
              // console.log();
              this.result = response.data;
              this.loading = false;
            });
        })
        .catch(error => {
          console.log(error);
          this.loading = false;
        });
    }
  }
};
</script>

<style>
#app {
  background: #00000094;
  width: 1100px;
  height: 600px;
}
</style>
