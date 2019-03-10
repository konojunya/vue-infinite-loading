<template>
  <div id="app">
    <h1>hello</h1>
    <div v-for="(item, $index) in list" :key="$index">
      <li style="list-style: none;">
        <p>{{item.title}}</p>
        <img :src="item.url" alt>
      </li>
    </div>
    <infinite-loading @infinite="infiniteHandler"></infinite-loading>
  </div>
</template>

<script>
import InfiniteLoading from "vue-infinite-loading";
const api = "https://wfc-2019.firebaseapp.com/images?";
const params = new URLSearchParams();
params.set("limit", "5");

export default {
  name: "app",
  components: {
    InfiniteLoading
  },
  data() {
    return {
      page: 0,
      list: []
    };
  },
  methods: {
    infiniteHandler($state) {
      params.set("offset", this.page);
      fetch(`${api}${params.toString()}`)
        .then(res => res.json())
        .then(res => {
          if (res.data.images.length) {
            this.page += 5;
            this.list.push(...res.data.images);
            $state.loaded();
          } else {
            $state.complete();
          }
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
