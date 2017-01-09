<template>
  <div :class="{'has-error': invalid}" class="form-inline">
    <label :for="variable.name" class="control-label">{{ variable.label }}</label><br>
    <datepicker ref="input" v-model="date" inputClass="form-control"></datepicker>
    <button class="btn btn-default" type="button" @click="clear">Clear</button>
  </div>
</template>

<script>
import Datepicker from 'vuejs-datepicker'

export default{
  props: ['value', 'variable'],
  data () {
    return {
      invalid: null,
      date: null,
      displayValue: null
    }
  },
  methods: {
    clear () {
      this.date = null
    },
    setTextValue (value) {
      if (value) {
        let splits = value.split('-')
        this.date = new Date(parseInt(splits[0]), parseInt(splits[1]) - 1, parseInt(splits[2]))
      } else {
        this.date = null
      }
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
    this.setTextValue(this.value)
    this.updateInvalid(this.value)
  },
  watch: {
    value (newValue) {
      if (this.displayValue !== newValue) {
        this.setTextValue(newValue)
      }
    },
    date (newValue) {
      let str = ''
      if (newValue) {
        str = (new Date(newValue.getTime() - (newValue.getTimezoneOffset() * 60000)).toISOString()).substr(0, 10)
      }
      this.displayValue = str
      this.$emit('input', str)
      this.updateInvalid(str)
    }
  },
  components: {
    Datepicker
  }
}
</script>

<style scoped>
.datepicker {
  display: inline-block;
}

</style>
