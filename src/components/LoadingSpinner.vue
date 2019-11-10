<template>
  <div v-if="loading" class="loading-spinner">
    <b-spinner label="label" />
  </div>
</template>

<script>
export default {
  name: 'LoadingSpinner',
  props: {
    loading: {
      type: Boolean,
      default: false
    },
    label: {
      type: String,
      default: 'Loading...'
    }
  }
}
</script>

<style scoped>
</style>
