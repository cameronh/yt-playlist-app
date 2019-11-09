<template>
  <div id="input-form">
    <form>
      <input v-model="url" type="text" class="form-control" placeholder="Enter a YouTube playlist URL.." />
    </form>
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
