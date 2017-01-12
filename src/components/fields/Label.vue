<template>
    <label :for="variable.name" class="control-label">{{ variable.label }}{{ variable.required == true ? '*' : '' }}</label>
</template>

<script>
export default{
  props: ['variable']
}
</script>
