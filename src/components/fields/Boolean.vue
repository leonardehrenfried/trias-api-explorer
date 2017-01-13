<template>
  <div :class="{'has-error': invalid}">
    <FieldLabel :variable="variable"></FieldLabel>
    <select ref="input" :value="value" @change="updateValue($event.target.value)" class="form-control">
      <option value="">--</option>
      <option value="true">true</option>
      <option value="false">false</option>
    </select>
  </div>
</template>

<script>
import FieldLabel from './FieldLabel'

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
  },
  components: {
    FieldLabel
  }
}
</script>
