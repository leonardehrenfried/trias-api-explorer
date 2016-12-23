<template>
    <input type="text" ref="input" :value="value" @input="updateValue($event.target.value)" class="form-control">
</template>

<script>
export default{
  props: ['value'],
  methods: {
    updateValue (value) {
      let validatedValue = value.replace(/[^0-9]/, '')
      if (validatedValue !== value) {
        this.$refs.input.value = validatedValue
      }
      this.$emit('input', validatedValue)
    }
  }
}
</script>
