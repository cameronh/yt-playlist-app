<template>
  <div class="input-form">
    <b-form-input v-model="input" :size="size" :class="classes" :placeholder="placeholder" />
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
      input: null
    }
  },
  watch: {
    'input': function (newVal, oldVal) {
      if (newVal !== oldVal) {
        this.$emit('input-changed', newVal);
      }
    }
  }
}
</script>

<style scoped>
</style>
