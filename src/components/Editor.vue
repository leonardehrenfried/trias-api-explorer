<template>
  <div v-if="template">
    <div v-for="variable in template.variables" class="form-group">
      <string v-if="variable.type === 'string'" :variable="variable" v-model="data[variable.name]" @invalidChanged="invalidChanged"></string>
      <integer v-else-if="variable.type === 'integer'" :variable="variable" v-model="data[variable.name]" @invalidChanged="invalidChanged"></integer>
      <enumeration v-else-if="variable.type === 'enumeration'" :variable="variable" v-model="data[variable.name]" @invalidChanged="invalidChanged"></enumeration>
      <boolean v-else-if="variable.type === 'boolean'" :variable="variable" v-model="data[variable.name]" @invalidChanged="invalidChanged"></boolean>
      <date-field v-else-if="variable.type === 'date'" :variable="variable" v-model="data[variable.name]" @invalidChanged="invalidChanged"></date-field>
      <!--
      <datetime v-if="variable.type === 'integer'" :variable="variable" @change="change"></datetime>
      -->
    </div>
    <button @click="send" :disabled="isInvalid" class="btn btn-success">Send</button>
    <button @click="reset" class="btn btn-danger">Clear</button>
  </div>
</template>

<script>
import String from './fields/String'
import Integer from './fields/Integer'
import Enumeration from './fields/Enumeration'
import Boolean from './fields/Boolean'
import DateField from './fields/DateField'

import every from 'lodash/every'

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
        newData[variable.name] = variable.default ? variable.default : ''
      })
      this.data = newData
      this.invalid = {}
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
    DateField
  }
}
</script>
