<template>
  <div class="tree-menu">
    <div class="label-wrapper">
      <div :style="indent" class="labelClasses">
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
  props: [ 'type', 'label', 'depth' ],
  computed: {
    indent() {
      return { transform: `translate(${this.depth * 50}px)` }
    }
  },
}
</script>

<style scoped >
  .label-wrapper {
    padding: 10px;
    cursor: default;
    border-bottom: 1px solid #ccc;
  }
</style>