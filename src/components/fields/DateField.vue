<template>
  <div :class="{'has-error': invalid}" class="form-inline">
    <Label :variable="variable"></Label>
    <datepicker ref="input" v-model="date" inputClass="form-control"></datepicker>
    <button class="btn btn-default" type="button" @click="clear">Clear</button>
  </div>
</template>

<script>
import Label from './Label'

import Datepicker from 'vuejs-datepicker'
import moment from 'moment'

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
        this.date = moment(value).toDate()
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
        str = moment(newValue).format('YYYY-MM-DD')
      }
      this.displayValue = str
      this.$emit('input', str)
      this.updateInvalid(str)
    }
  },
  components: {
    Datepicker,
    Label
  }
}
</script>

<style scoped>
.datepicker {
  display: inline-block;
}

</style>
