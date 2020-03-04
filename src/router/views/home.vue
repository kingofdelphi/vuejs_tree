<script>
import appConfig from '@src/app.config'
import Layout from '@layouts/main.vue'
import Input from './custom-input/index.vue'
import CSelect from './select/index.vue'
import Root from './tree/Root.vue'

export default {
  components: { Layout, Input, CSelect, Root },
  page: {
    title: 'Home',
    meta: [{ name: 'description', content: appConfig.description }],
  },
  data: function() {
    return {
      selected: '1',
      value: '2',
      form: {
        inputURL: '',
      },
      item: {
        root: true,
        label: 'Root',
        children: [
          {
            label: 'Test1',
            children: [
              { label: 'child1', children: [{ label: 'save me' }] },
              { label: 'fuck' },
            ],
          },
          { label: 'Test2' },
        ],
      },
    }
  },
  methods: {
    submit: function(e) {
      console.log({ url: this.form.inputURL, value: this.value })
    },
    reset: function() {
      this.selected = '1'
    },
    updateSelect: function(value) {
      this.selected = value
    },
  },
}
</script>
<template>
  <Layout>
    <h1>Tiny Url Clone</h1>
    <div>
    <h3>Tree 1</h3>
    <Root :node-id="[]" :item="item" />
    </div>
    <div>
    <h3>Tree 2</h3>
    <Root :node-id="[]" :item="{ root: true, label: 'Tree2', children: [{ label: 'Test' }] }" />
    </div>
    <label>
      Custom select
      <CSelect
        :selected="selected"
        :options="[
          { value: 1, text: 'Ktm' },
          { value: 2, text: 'Pokhara' },
        ]"
        @update="updateSelect"
      />
      <button @click="reset">Reset</button>
    </label>
    <form :class="$style.form" @submit.prevent="submit">
      <label>
        URL
        <Input v-model="form.inputURL" placeholder="Enter URL to minify" />
      </label>
      <select v-model="value">
        <option>1</option>
        <option>2</option>
        <option>3</option>
      </select>
      <button>Submit</button>
    </form>
  </Layout>
</template>

<style lang="scss" module>
.form {
  display: flex;
  flex-direction: column;
  button {
    padding: 20px 20px;
  }
}
</style>
