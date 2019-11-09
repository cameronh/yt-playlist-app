<template>
  <div id="app">
    <div class="container">
      <InputForm @url-changed="processURL" />
      <div v-if="state.loading" class="spinner-border" role="status">
        <span class="sr-only">Loading...</span>
      </div>

      <ItemContainer :items="state.items" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import InputForm from './components/InputForm';
import ItemContainer from './components/ItemContainer';

export default {
  name: "app",
  data: () => {
    return {
      state: {
        loading: false,
        url: null,
        error: null,
        items: {}
      }
    }
  },
  methods: {
    async processURL(url) {
      this.state.url = url;

      try {
        this.state.loading = true;
        const results = await axios.get('http://localhost:3000/playlist', {
          params: {
            id: url
          }
        });
        if (results.status == '200' && results.data) {
          // eslint-disable-next-line no-console
          console.log(results.data);
          this.state.items = results.data;
        }

        this.state.loading = false;
      } catch (err) {
        this.state.error = err;
        this.state.loading = false;
      }
    }
  },
  components: {
    InputForm,
    ItemContainer
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
