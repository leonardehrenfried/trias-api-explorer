<template>
  <div :class="{'has-error': invalid}">
    <Label :variable="variable"></Label>
    <input type="text" ref="input" :value="value" @input="updateValue($event.target.value)" class="form-control">
  </div>
</template>

<script>
import Label from './Label'

export default{
  props: ['value', 'variable'],
  data () {
    return {
      invalid: null
    }
  },
  methods: {
    updateValue (newValue) {
      let validatedValue = newValue.replace(/[^0-9]/, '')
      if (validatedValue !== newValue) {
        this.$refs.input.value = validatedValue
      }
      this.$emit('input', validatedValue)
      this.updateInvalid(validatedValue)
    },
    updateInvalid (value) {
      let invalid = !!this.variable.required && value === ''
      if (invalid !== this.invalid) {
        this.invalid = invalid
        this.$emit('invalidChanged', this.variable.name, invalid)
      }
    }
  },
  created () {
    this.updateInvalid(this.value)
  },
  watch: {
    value (newValue) {
      this.updateInvalid(newValue)
    }
  },
  components: {
    Label
  }
}
</script>
