<template>
  <div class="tree-menu" v-if="type === 'directory'">
    <div class="label-wrapper" @click="toggleChildren">
      <div :style="indent" class="labelClasses">
        <img src="../assets/folder_icon.png" width="20" class="img--folder"/>
        {{ label }}
      </div>
    </div>
    <div 
      v-if="showChildren"
    >
      <Catalog 
        v-for="(node, index) in nodes" 
        :key="`${node.name}-${index}`"
        :nodes="node.contents" 
        :label="node.name"
        :depth="depth + 1"
        :type="node.type"  
      />
    </div>
  </div>
  <div v-else>
    <Element  
      :key="label" 
      :label="label"
      :depth="depth"
      :type="type"  
    />
  </div>
</template>

<script>
import Element from './Element.vue';

export default {
  name: 'Catalog',
  props: [ 'nodes', 'label', 'depth', 'type' ],
  components: {
    Element
  },
  data() {
     return {
       showChildren: false
     }
  },
  computed: {
    indent() {
      return { transform: `translate(${this.depth * 50}px)` }
    }
  },
  methods: {
    toggleChildren() {
       this.showChildren = !this.showChildren;
    }
  }
}
</script>

<style>
  .label-wrapper {
    padding: 10px;
    cursor: pointer;
    border-bottom: 1px solid #ccc;
  }
  .labelClasses {
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .img--folder {
    margin-left: 8px;
  }
</style>
