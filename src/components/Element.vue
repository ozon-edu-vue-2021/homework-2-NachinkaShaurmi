<template>
  <div class="tree-menu">
    <div class="label-wrapper" @click="elementClick">
      <div :style="indent" :class="{labelClasses: true, active: path === currentPath}">
        <img src="../assets/link_icon.png" v-show="type === 'link'" width="20"/>
        <img src="../assets/file_icon.png" v-show="type === 'file'" width="32"/>
        {{ label }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Element',
  props: [ 'type', 'label', 'depth', 'path', 'currentPath' ],
  computed: {
    indent() {
      return { transform: `translate(${this.depth * 50}px)` }
    }
  },
  methods: {
    elementClick() {
      this.$root.$emit('childClick', this.$props.path);
    }
  }
}
</script>

<style scoped >
  .label-wrapper {
    padding: 10px;
  }
</style>