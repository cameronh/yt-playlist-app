<template>
  <div id="app" class="bg-dark">
    <b-container>
      <b-row class="mt-5">
        <b-col>
          <InputForm @url-changed="processURL" />
        </b-col>
      </b-row>
      <b-row class="mt-5">
        <b-col>
          <ItemContainer @download-video="downloadVideo" :items="state.items" :loading="state.loading"/>
        </b-col>
      </b-row>
    </b-container>
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
      if (!url.match(/^.*(youtu.be\/|list=)([^#&?]*).*/)) return;
      try {
        this.state.loading = true;
        const results = await axios.get('http://localhost:3000/playlist', {
          params: {
            id: url
          }
        });
        if (results.data && results.data.success) {
          // eslint-disable-next-line no-console
          // console.log(results.data);
          this.state.items = results.data;
        }

        this.state.loading = false;
      } catch (err) {
        this.state.error = err;
        this.state.loading = false;
      }
    },
    async downloadVideo(video) {
      try {
        this.state.loading = true;
        const results = await axios({
          url: 'http://localhost:3000/download',
          params: {
            id: video
          },
          method: 'GET'
        });
        if (results.data && results.data.success) {
          let fileURL = window.URL.createObjectURL(new Blob([Buffer.from(results.data.video, 'base64')]));
          let fileLink = document.createElement('a');
          fileLink.href = fileURL;
          fileLink.setAttribute('download', 'video.mp4');
          document.body.appendChild(fileLink);

          fileLink.click();

          this.state.loading = false;
        }
      } catch (err) {
        this.state.error = err;
        this.state.downloading = false;
      }
    }
  },
  components: {
    InputForm,
    ItemContainer,
  }
};
</script>

<style>
html {
  background-color: #343a40;
}
</style>
