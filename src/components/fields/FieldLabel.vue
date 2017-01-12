<template>
  <div>
    <label :for="variable.name" class="control-label">{{ variable.label }}{{ variable.required === true ? '*' : '' }}</label>
    <div v-if="variable.description">{{ variable.description }}</div>
  </div>
</template>

<script>
export default{
  props: ['variable']
}
</script>
