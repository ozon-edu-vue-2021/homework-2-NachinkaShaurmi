<template>
  <div id="app">
    <div class="path-wrapper">
      <h2>{{ currentPath.slice(0, -1) }}</h2>
    </div>
    <div v-if="info" class="main">
      <Catalog
        :nodes="info.data.contents"
        :depth="0"
        :path="'node_modules/'"
        :label="info.data.name"
        :type="info.data.type"
        :currentPath="currentPath"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Catalog from "./components/Catalog.vue";

export default {
  name: "App",
  components: {
    Catalog,
  },
  data: () => ({
    currentPath: "",
    rootEl: null,
    isShow: false,
    info: null,
    currentType: "",
  }),
  methods: {
    keyDownHandler(e) {
      if (this.rootEl) {
        const next = this.rootEl.nextElementSibling;
        const prev = this.rootEl.previousElementSibling;
        const parent = this.rootEl?.parentElement?.parentElement;
        const firstChild = this.rootEl?.children[1]?.firstElementChild;

        switch (e.key) {
          case "ArrowDown":
            if (next) {
              this.currentPath = next.getAttribute("path");
              this.rootEl = next;
            }
            break;

          case "ArrowUp":
            if (prev) {
              this.currentPath = prev.getAttribute("path");
              this.rootEl = prev;
            }
            break;

          case "ArrowLeft":
            if (parent) {
              if (this.currentPath === "node_modules/") return;
              this.currentPath = parent.getAttribute("path");
              this.rootEl = parent;
            }
            break;

          case "ArrowRight":
            if (firstChild) {
              this.currentPath = firstChild.getAttribute("path");
              this.rootEl = firstChild;
            }
            break;

          default:
            break;
        }
      }
    },
  },
  mounted() {
    axios
      .get("./static/node_modules.json")
      .then((response) => (this.info = response));

    this.$root.$on("childClick", (p, el, type) => {
      this.rootEl = el;
      this.currentType = type;
      this.currentPath = p;
    });

    window.addEventListener("keyup", this.keyDownHandler);
  },
  destroyed() {
    window.removeEventListener("keydown", this.keyDownHandler);
  },
};
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
