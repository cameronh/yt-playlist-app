<template>
  <div>
    <b-alert
      dismissable
      fade
      :show="showAlert"
      :variant="variant"
      @dismissed="dissmissCountDown=0"
      @dismiss-count-down="countDownChanged"
    >{{ this.text }}</b-alert>
  </div>
</template>

<script>
export default {
  name: "ErrorAlert",
  props: {
    text: {
      type: String,
      default: "Error"
    },
    variant: {
      type: String,
      default: "danger"
    },
    dismissSeconds: {
      type: Number,
      default: 3
    }
  },
  data() {
    return {
      dismissCountDown: 0
    }
  },
  computed: {
    showAlert() {
      return (this.text && this.text.length > 0) && this.dismissCountDown;
    }
  },
  watch: {
    'text': function(newVal) {
      if (newVal.length > 0) {
        this.dismissCountDown = this.dismissSeconds;
      }
    }
  },
  methods: {
    countDownChanged(dismissCountDown) {
      this.dismissCountDown = dismissCountDown;
    },
  }
};
</script>

<style scoped>
</style>
