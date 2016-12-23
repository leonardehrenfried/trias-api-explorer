<template>
    <div v-if="template">
        <div v-for="variable in template.variables" class="form-group">
            <label :for="variable.name">{{ variable.label }}</label>
            <string v-if="variable.type === 'string'" :variable="variable" v-model="data[variable.name]"></string>
            <integer v-else-if="variable.type === 'integer'" :variable="variable" v-model="data[variable.name]"></integer>
          <!--
          <date v-if="variable.type === 'integer''" :variable="variable" v-model="data[variable.name]"></date>
          <datetime v-if="variable.type === 'integer''" :variable="variable" @change="change"></datetime>
          <enumeration v-if="variable.type === 'integer''" :variable="variable" @change="change"></enumeration>
          <boolean v-if="variable.type === 'integer''" :variable="variable" @change="change"></boolean>-->
        </div>

        <button @click="reset">Clear</button>
        <button @click="send">Send</button>
    </div>
</template>

<script>
import String from './fields/String'
import Integer from './fields/Integer'

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
    Integer
  }
}
</script>
