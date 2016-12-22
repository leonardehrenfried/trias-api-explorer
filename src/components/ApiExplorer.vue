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
        <div v-if="currentTemplate">{{ currentTemplate.name }}</div>
        <pre>{{ xmlTemplate }}</pre>
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
      templateIndex: null,
      xmlTemplate: ''
    }
  },

  created () {
    fetch('/static/api_templates.json')
      .then((response) => response.json())
      .then((json) => {
        this.templates = json
        this.templateIndex = 0
      })
      .catch((ex) => (console.log('parsing api_templates failed', ex)))
  },

  watch: {
    templateIndex (newIndex) {
      this.currentTemplate = this.templates[this.templateIndex]
      fetch('/static/' + this.currentTemplate.template)
          .then((response) => response.text())
          .then((xml) => (this.xmlTemplate = xml))
          .catch((ex) => (console.log(`loading of xml template ${this.currentTemplate.template} failed`)))
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
