<template>
  <div id="app">
    <form @submit.prevent="searchGifs">
      <label for="search">Search</label>
      <input type="text" v-model="search" placeholder="search term" />
      <button type="submit" class="btn">Search</button>
    </form>
    <div class="loading" v-if="loading">
      <h1>Loading....</h1>
    </div>
    <div class="images" v-if="!loading">
      <img v-for="gif in gifs" :src="gif" :key="gif" />
    </div>
  </div>
</template>

<script>
import { value } from "vue-function-api";
const API_URL =
  "https://api.giphy.com/v1/gifs/search?api_key=JnxTmEGKXjZeUKBzRjTQoMDg8OX8pS5U&rating=pg&q=";

export default {
  setup() {
    const search = value("");
    const loading = value(false);
    const gifs = value([]);
    return {
      search,
      searchGifs,
      loading,
      gifs
    };
    async function searchGifs() {
      const gif = `${API_URL}${search.value}`;
      loading.value = true;

      const request = await fetch(gif);
      const { data } = await request.json();
      search.value = "";
      gifs.value = data.map(
        ({
          images: {
            fixed_height: { url }
          }
        }) => url
      );
      setTimeout(() => {
        loading.value = false;
      }, 2000);
      // return data;
    }
  }
};
</script>

<style>
img {
  width: 100%;
}
.images {
  column-count: 4;
}
.btn {
  width: auto;
  padding: 10px;
}
</style>
