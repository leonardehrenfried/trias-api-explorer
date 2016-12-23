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
      valid: true
    }
  },
  methods: {
    updateValue (value) {
      this.$emit('input', value)
    }
  },
  computed: {
    invalid () {
      return this.variable.required && this.value === ''
    }
  }
}
</script>
