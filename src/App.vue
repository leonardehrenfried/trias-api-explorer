<template>
  <div id="app">
    <api-explorer />
  </div>
</template>

<script>
import ApiExplorer from './components/ApiExplorer'

import 'bootstrap/dist/css/bootstrap.css'

export default {
  name: 'app',
  components: {
    ApiExplorer
  }
}
</script>
