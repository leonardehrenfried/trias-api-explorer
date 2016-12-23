<template>
  <div>
    <label :for="variable.name">{{ variable.label }}</label>
    <input type="text" ref="input" :value="value" @input="updateValue($event.target.value)" class="form-control">
  </div>
</template>

<script>
export default{
  props: ['value', 'variable'],
  methods: {
    updateValue (value) {
      this.$emit('input', value)
    }
  }
}
</script>
