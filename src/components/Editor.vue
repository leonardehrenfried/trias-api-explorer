<template>
  <div v-if="template">
    <div v-for="variable in template.variables" class="form-group">
      <string v-if="variable.type === 'string'" :variable="variable" v-model="data[variable.name]" @invalidChanged="invalidChanged"></string>
      <integer v-else-if="variable.type === 'integer'" :variable="variable" v-model="data[variable.name]" @invalidChanged="invalidChanged"></integer>
      <enumeration v-else-if="variable.type === 'enumeration'" :variable="variable" v-model="data[variable.name]" @invalidChanged="invalidChanged"></enumeration>
      <boolean v-else-if="variable.type === 'boolean'" :variable="variable" v-model="data[variable.name]" @invalidChanged="invalidChanged"></boolean>
      <date-field v-else-if="variable.type === 'date'" :variable="variable" v-model="data[variable.name]" @invalidChanged="invalidChanged"></date-field>
      <datetime v-if="variable.type === 'datetime'" :variable="variable" v-model="data[variable.name]" @invalidChanged="invalidChanged"></datetime>
    </div>
    <button @click="send" :disabled="isInvalid" class="btn btn-success">Send request</button>
    <button @click="reset" class="btn btn-danger">Reset form</button>
  </div>
</template>

<script>
import String from './fields/String'
import Integer from './fields/Integer'
import Enumeration from './fields/Enumeration'
import Boolean from './fields/Boolean'
import DateField from './fields/DateField'
import Datetime from './fields/Datetime'

import every from 'lodash.every'
import moment from 'moment'

export default {
  props: ['template'],
  data () {
    return {
      data: {},
      invalid: {}
    }
  },

  methods: {
    send () {
      this.$emit('send')
    },
    reset () {
      let newData = {}
      this.template.variables.forEach((variable) => {
        if (variable.default) {
          newData[variable.name] = variable.default
        } else {
          if (variable.type === 'date') {
            newData[variable.name] = moment().format('YYYY-MM-DD')
          } else if (variable.type === 'datetime') {
            newData[variable.name] = moment().format('YYYY-MM-DDTHH:mm:ss')
          } else {
            newData[variable.name] = ''
          }
        }
      })
      this.data = newData
      this.invalid = {}
      this.$emit('reset')
    },
    invalidChanged (variable, invalid) {
      this.$set(this.invalid, variable, invalid)
    }
  },

  watch: {
    template (newTemplate) {
      this.reset()
    },
    data (newData) {
      this.$emit('change', newData)
    }
  },

  computed: {
    isInvalid () {
      return !every(this.invalid, (value, key) => value === false)
    }
  },

  components: {
    String,
    Integer,
    Enumeration,
    Boolean,
    DateField,
    Datetime
  }
}
</script>
