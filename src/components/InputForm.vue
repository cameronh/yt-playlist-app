<template>
  <div id="input-form">
    <b-form-input id="url" v-model="url" :size="size" :class="classes" :placeholder="placeholder" />
  </div>
</template>

<script>
export default {
  name: 'InputForm',
  props: {
    size: {
      type: String,
      default: 'lg'
    },
    classes: {
      type: String,
      default: 'bg-dark text-white'
    },
    placeholder: {
      type: String,
      default: ''
    }
  },
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
