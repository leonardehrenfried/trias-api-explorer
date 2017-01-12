<template>
  <div :class="{'has-error': invalid}">
    <Label :variable="variable"></Label>
    <select ref="input" :value="value" @input="updateValue($event.target.value)" class="form-control">
      <option value="">--</option>
      <option v-for="option in variable.options" :value="option">{{ option }}</option>
    </select>
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
    Label
  }
}
</script>
