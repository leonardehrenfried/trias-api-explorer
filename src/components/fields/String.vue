<template>
    <input ref="input" :value="value" @input="updateValue($event.target.value)" class="form-control">
</template>

<script>
export default{
  props: ['value'],
  methods: {
    updateValue (value) {
      this.$emit('input', value)
    }
  }
}
</script>
