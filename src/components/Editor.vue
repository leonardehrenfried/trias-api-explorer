<template>
    <div v-if="template">
        <div v-for="variable in template.variables">
          {{ variable.name }}
        </div>

        <button @click="send">Send</button>
    </div>
</template>

<script>
export default {
  props: ['template'],
  data () {
    return {
      data: {
        stop_point_ref: ''
      }
    }
  },
  methods: {
    send () {
      console.log('click')
    }
  }
}
</script>
