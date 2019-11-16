<template>
  <span v-if="loading" class="loading-spinner">
    <b-spinner :small="small" :variant="variant" :label="label" style="margin-bottom: 2px" />
  </span>
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
    },
    small: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style scoped>
</style>
