<template>
  <select 
    class="select"
    :value="modelValue" 
    @change="changeOptions">
    <option 
      v-for="option in options" 
      :key="option.value" 
      :value="option.value"
      :selected="option.selected">
    {{option.name}}
  </option>
  </select>
</template>

<script>
export default {
  name: 'my-select',

  props: {
    modelValue: {
      type: String
    },
    options: {
      type: Array,
      default:() => []
    }
  },

  methods: {
    changeOptions(event) {
      this.$emit('update:modelValue', event.target.value)
    }
  }
}
</script>

<style>
.select {
  background: none;
  border: 1px solid teal;
  color: teal;
  padding: 5px;
}
</style>