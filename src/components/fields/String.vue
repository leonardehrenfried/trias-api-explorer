<template>
  <div :class="{'has-error': invalid}">
    <label :for="variable.name" class="control-label">{{ variable.label }}</label>
    <input type="text" ref="input" :value="value" @input="updateValue($event.target.value)" class="form-control">
  </div>
</template>

<script>
export default{
  props: ['value', 'variable'],
  data () {
    return {
      invalid: null
    }
  },
  methods: {
    updateValue (newValue) {
      this.$emit('input', newValue)
      this.updateInvalid(newValue)
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
  }
}
</script>
