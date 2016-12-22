<template>


<div class="container">

  <div class="row">
    <div class="col-xs-12 form-inline">
      Template:
      <select class="form-control" v-model="templateIndex">
        <option v-for="(template, index) in templates" :value="index">{{ template.name }}</option>
      </select>
    </div>
  </div>

  <div class="row">
    <div class="col-xs-4">
      Editor:
    </div>
    <div class="col-xs-8">
      <div>xml editor</div>
      <div>xml output</div>
    </div>
  </div>
</div>


</template>

<script>
import promise from 'es6-promise'
promise.polyfill()
import 'isomorphic-fetch'

export default {
  data () {
    return {
      templates: [],
      templateIndex: 0
    }
  },

  computed: {
    currentTemplate: () => this.templates[this.templateIndex]
  },

  created () {
    fetch('/static/api_templates.json')
      .then((response) => response.json())
      .then((json) => (this.templates = json))
      .catch((ex) => (console.log('parsing api_templates failed', ex)))
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
