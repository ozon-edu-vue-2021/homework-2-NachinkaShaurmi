<template>
  <div class="tree-menu" v-if="type === 'directory'">
    <div class="label-wrapper" @click="toggleChildren">
      <div :style="indent" :class="{labelClasses: true, active: isActive}">
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
        :path="path + node.name + '/'"
        @childClick="childClick"  
      />
    </div>
  </div>
  <div v-else>
    <Element  
      :key="label" 
      :label="label"
      :depth="depth"
      :type="type"
      :path="path"
      @childClick="$emit('childClick', path)"
    />
  </div>
</template>

<script>
import Element from './Element.vue';

export default {
  name: 'Catalog',
  props: [ 'nodes', 'label', 'depth', 'type', 'path' ],
  components: {
    Element
  },
  data() {
     return {
       showChildren: false,
       isActive: false,
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
       this.isActive = !this.isActive;
       this.$emit('childClick', this.$props.path);
    },
    childClick(param) {
      // if need clear active class uncomment next line
      // this.isActive = false;
      this.$emit('childClick', param);
    },
  }
}
</script>

<style>
  .label-wrapper {
    padding: 10px;
  }
  .labelClasses {
    min-height: 32px;
    display: flex;
    align-items: center;
    gap: 10px;
    cursor: pointer;
    border-radius: 10px;
    transition: .3s;
  }
  .labelClasses.active {
    background-color: mediumturquoise;
  }
  .labelClasses:hover {
    background-color: #ccc;
  }
  .img--folder {
    margin-left: 8px;
  }
</style>
