<template>
  <div :class="{'has-error': invalid}">
    <label :for="variable.name" class="control-label">{{ variable.label }}</label><br>
    <div class="datetime-container">
      <datepicker v-model="date" inputClass="form-control"></datepicker>
      <vue-timepicker v-model="time" format="HH:mm:ss" hide-clear-button></vue-timepicker>
      <button class="btn btn-default" type="button" @click="clear">Clear</button>
    </div>
  </div>
</template>

<script>
import Datepicker from 'vuejs-datepicker'
import VueTimepicker from './TimePicker'
import moment from 'moment'

import padStart from 'lodash.padstart'

export default{
  props: ['value', 'variable'],
  data () {
    return {
      invalid: null,
      date: null,
      time: null,
      displayValue: null
    }
  },
  methods: {
    clear () {
      this.setTextValue(null)
    },
    setTextValue (value) {
      if (value) {
        let date = moment(value)
        this.date = date.toDate()
        this.time = {
          HH: padStart(date.hour().toString(), 2, '0'),
          mm: padStart(date.minute().toString(), 2, '0'),
          ss: padStart(date.second().toString(), 2, '0')
        }
      } else {
        this.date = null
        this.time = {
          HH: '',
          mm: '',
          ss: ''
        }
      }
    },
    updateInvalid (value) {
      let invalid = !!this.variable.required && value === ''
      if (invalid !== this.invalid) {
        this.invalid = invalid
        this.$emit('invalidChanged', this.variable.name, invalid)
      }
    },
    updateDateTime () {
      let str = ''
      if (this.date && this.time.HH !== '' && this.time.mm !== '' && this.time.ss !== '') {
        let dt = moment(this.date)
        dt.hour(parseInt(this.time.HH))
        dt.minute(parseInt(this.time.mm))
        dt.second(parseInt(this.time.ss))
        str = dt.format('YYYY-MM-DDTHH:mm:ss')
      }
      this.displayValue = str
      this.$emit('input', str)
      this.updateInvalid(str)
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
    date () {
      // date picker changed
      this.updateDateTime()
    },
    time () {
      // time picker changed
      this.updateDateTime()
    }
  },
  components: {
    Datepicker,
    VueTimepicker
  }
}
</script>

<style lang="scss" scoped>
.datetime-container {
  display: flex;
  .time-picker, button {
    margin-left: 5px;
  }
}
</style>
