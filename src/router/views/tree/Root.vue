<script>
import { mapState } from 'vuex'
import Vue from 'vue'
import TreeNode from './index.vue'

const tree = (data) => ({
  state: { data },
  namespaced: true,
  mutations: {
    setNodeData: (state, indexes) => {
      // const newstatedata = JSON.parse(JSON.stringify(state.data))
      let node = state.data
      for (let i = 0; i < indexes.length; ++i) {
        node = node.children[indexes[i]]
      }
      Vue.set(node, 'collapsed', !node.collapsed)
    },
    addItem: (state, indexes) => {
      let node = state.data
      console.log('for ', indexes)
      for (let i = 0; i < indexes.length; ++i) {
        node = node.children[indexes[i]]
      }
      if (!node.children) {
        Vue.set(node, 'children', [])
      }
      Vue.set(node, 'collapsed', false)
      node.children.push({
        label: 'hack',
      })
    },
  },
})

export default {
  components: { TreeNode },
  props: {
    item: Object,
    nodeId: {
      type: Array,
      default: () => [],
    },
  },
  data: () => ({
    namespace: 'tree' + new Date().getTime(),
  }),
  computed: {
    ...mapState({
      newItem(state) {
        return state[this.namespace].data
      },
    }),
  },
  created: function() {
    console.log(this.namespace)
    this.$store.registerModule(this.namespace, tree({ ...this.item }))
  },
  methods: {
    toggleCollapse: function(nodeId) {
      console.log('toggle main', nodeId)
      this.$store.commit(`${this.namespace}/setNodeData`, nodeId || this.nodeId)
    },
    addItem: function(nodeId) {
      console.log('add main', nodeId)
      this.$store.commit(`${this.namespace}/addItem`, nodeId || this.nodeId)
    },
  },
}
</script>

<template>
  <div :class="{ [$style.indent]: true }">
    <TreeNode
      :node-id="[]"
      :item="newItem"
      @addItem="addItem"
      @toggleCollapse="toggleCollapse"
    />
  </div>
</template>

<style lang="scss" module>
@import '@design';
</style>
