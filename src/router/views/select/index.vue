<script>
export default {
  props: ['selected', 'options'],
  data: function() {
    return {
      ptions: undefined,
    }
  },
  computed: {
    ncselected: {
      get() {
        console.log('get')
        return this.selected
      },
      set(value) {
        console.log('new prop')
        return value
      },
    },
  },
  methods: {
    onChange: function(event) {
      console.log('emit')
      this.$emit('update', event.target.value)
    },
  },
}
</script>

<template>
  <select v-model="ncselected" @change="onChange($event)">
    <option v-for="option in options" :key="option.value" :value="option.value">
      {{ option.text }}
    </option>
  </select>
</template>

<style lang="scss" module>
@import '@design';
</style>
