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
        Stop Point Ref: <input type="text" v-model="data.stop_point_ref">
      </div>
      <div class="col-xs-8">
        <div v-if="currentTemplate">{{ currentTemplate.name }}</div>
        <pre v-html="highlightedTemplate"></pre>
        <div>xml output</div>
      </div>
    </div>
  </div>
</template>

<script>
import promise from 'es6-promise'
promise.polyfill()
import 'isomorphic-fetch'

import Prism from 'prismjs'
import 'prismjs/themes/prism.css'
import 'prismjs/components/prism-markup'

import nunjucks from 'nunjucks'

export default {
  data () {
    return {
      templates: [],
      templateIndex: null,
      currentTemplate: null,
      xmlTemplate: '',
      data: {
        stop_point_ref: 'default'
      }
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
    nunjucks.configure({trimBlocks: true, lstripBlocks: true})
  },

  watch: {
    templateIndex (newIndex) {
      this.currentTemplate = this.templates[this.templateIndex]
      fetch('/static/' + this.currentTemplate.template)
          .then((response) => response.text())
          .then((xml) => (this.xmlTemplate = xml))
          .catch((ex) => (console.log(`loading of xml template ${this.currentTemplate.template} failed`)))
    }
  },

  computed: {
    renderedTemplate () {
      return nunjucks.renderString(this.xmlTemplate, this.data)
    },
    highlightedTemplate () {
      return Prism.highlight(this.renderedTemplate, Prism.languages.markup)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
