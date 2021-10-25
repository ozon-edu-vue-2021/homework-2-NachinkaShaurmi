<template>
  <div id="app">
    <div v-if="info">
      <Catalog 
        :nodes="info.data.contents"
        :depth="0"   
        :label="info.data.name"
        :type="info.data.type"
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
      isShow: false,
      info: null
    }),
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
  margin: 0 auto;
  max-width: 1200px;
}
</style>
