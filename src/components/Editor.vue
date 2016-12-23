<template>
  <div v-if="template">
    <div v-for="variable in template.variables" class="form-group">
      <string v-if="variable.type === 'string'" :variable="variable" v-model="data[variable.name]"></string>
      <integer v-else-if="variable.type === 'integer'" :variable="variable" v-model="data[variable.name]"></integer>
      <enumeration v-else-if="variable.type === 'enumeration'" :variable="variable" v-model="data[variable.name]"></enumeration>
      <boolean v-else-if="variable.type === 'boolean'" :variable="variable" v-model="data[variable.name]"></boolean>
      <!--
      <date v-if="variable.type === 'integer''" :variable="variable" v-model="data[variable.name]"></date>
      <datetime v-if="variable.type === 'integer''" :variable="variable" @change="change"></datetime>
      -->
    </div>
    <button @click="reset">Clear</button>
    <button @click="send">Send</button>
  </div>
</template>

<script>
import String from './fields/String'
import Integer from './fields/Integer'
import Enumeration from './fields/Enumeration'
import Boolean from './fields/Boolean'

export default {
  props: ['template'],
  data () {
    return {
      data: {}
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

  components: {
    String,
    Integer,
    Enumeration,
    Boolean
  }
}
</script>
