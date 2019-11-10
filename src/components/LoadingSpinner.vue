<template>
  <div v-if="loading" class="loading-spinner">
    <b-spinner :variant="variant" :label="label" />
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
    },
    variant: {
      type: String,
      default: 'primary'
    }
  }
}
</script>

<style scoped>
</style>
