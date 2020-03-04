<script>
export default {
  name: 'TreeNode',

  props: {
    item: Object,
    nodeId: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    click: function() {
      this.$emit('toggleCollapse', this.nodeId)
    },
    addItem: function() {
      this.$emit('addItem', this.nodeId)
    },
    propToggleCollapse: function(nodeId) {
      console.log('prop collapse', nodeId)
      this.$emit('toggleCollapse', nodeId)
    },
    propAddItem: function(nodeId) {
      console.log('prop add', nodeId)
      this.$emit('addItem', nodeId)
    },
  },
}
</script>

<template>
  <div :class="{ [$style.indent]: true }">
    <div
      :class="{ [$style.root]: item.root === true, [$style.header]: true }"
      @click.stop="click"
    >
      <label
        >{{ item.label }}-{{ item.collapsed ? 'collapsed' : 'nocollap' }}</label
      >
      <button @click.stop="addItem">+</button>
    </div>
    <div
      v-for="(node, idx) in item.children"
      :key="idx"
      :class="{ [$style.collapsed]: item.collapsed }"
    >
      <TreeNode
        :node-id="nodeId.concat(idx)"
        :item="node"
        @addItem="propAddItem"
        @toggleCollapse="propToggleCollapse"
      />
    </div>
  </div>
</template>

<style lang="scss" module>
@import '@design';
.indent {
  margin-left: 40px;
}
div {
  cursor: pointer;
  box-shadow: 0 1px grey;
  &.root {
    background: silver;
  }
  &.collapsed {
    display: none;
  }
}
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  button {
    width: min-content;
  }
}
</style>
