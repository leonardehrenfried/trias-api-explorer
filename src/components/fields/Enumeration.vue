<template>
  <div :class="{'has-error': !valid}">
    <label :for="variable.name" class="control-label">{{ variable.label }}</label>
    <select ref="input" :value="value" @input="updateValue($event.target.value)" class="form-control">
      <option value="">--</option>
      <option v-for="option in variable.options" :value="option">{{ option }}</option>
    </select>
  </div>
</template>

<script>
export default{
  props: ['value', 'variable'],
  data () {
    return {
      valid: true
    }
  },
  methods: {
    updateValue (value) {
      if (this.variable.required && value === '') {
        this.valid = false
      } else {
        this.valid = true
      }
      this.$emit('input', value)
    }
  }
}
</script>
