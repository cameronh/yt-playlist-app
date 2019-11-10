<template>
  <div id="input-form">
    <b-form-input id="url" v-model="url" size="lg" class="bg-dark text-white" placeholder="Enter a YouTube playlist URL.." />
  </div>
</template>

<script>
export default {
  name: 'InputForm',
  data: () => {
    return {
      url: null
    }
  },
  watch: {
    'url': function (newVal, oldVal) {
      if (newVal !== oldVal) {
        this.$emit('url-changed', newVal);
      }
    }
  }
}
</script>

<style scoped>
</style>
