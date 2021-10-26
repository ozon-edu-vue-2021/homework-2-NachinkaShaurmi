<template>
  <div id="app">
    <div class="path-wrapper">
      <h2>{{currentPath}}</h2>
    </div>
    <div v-if="info" class="main">
      <Catalog 
        :nodes="info.data.contents"
        :depth="0"
        :path="'node_modules/'"   
        :label="info.data.name"
        :type="info.data.type"
        @childClick="changePath"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Catalog from './components/Catalog.vue';

export default {
  name: 'App',
  components: {
    Catalog
  },
  data: () => ({
      currentPath: "",
      isShow: false,
      info: null
    }),
  methods: {
    changePath(p) {
       this.currentPath = p.slice(0, -1);
    }
  },
  mounted() {
    axios
      .get('./static/node_modules.json')
      .then(response => (this.info = response));
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  padding-top: 20px;
  font-family: "Open Sans", sans-serif;
  font-size: 18px;
  font-weight: 300;
  line-height: 1em;
  padding-right: calc(20px - (100vw - 100%));
}

#app {
  margin: 60px auto;
  max-width: 1200px;
}

.path-wrapper {
  display: flex;
  align-items: center;
  padding: 10px;
  min-height: 50px;
  width: 100%;
  border: 2px solid black;
}

.main {
  margin-top: 20px;
}
</style>
