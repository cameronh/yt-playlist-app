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
          <ItemContainer
            @download-video="downloadVideo"
            :playlistInfo="state.playlistInfo"
            :items="state.items"
            :loading="state.loading"
            :downloading="state.downloading"
          />
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
      api_url: '',
      state: {
        loading: false,
        downloading: false,
        url: null,
        error: null,
        playlistInfo: {},
        items: []
      }
    }
  },
  mounted() {
    if (process.env.NODE_ENV === 'production') {
      this.api_url = 'https://chudson.io:3000';
    } else {
      this.api_url = 'http://localhost:3000';
    }
  },
  methods: {
    async processURL(url) {
      this.state.url = url;
      if (!url.match(/^.*(youtu.be\/|list=)([^#&?]*).*/)) return;
      try {
        this.state.loading = true;
        const results = await axios.get(`${this.api_url}/playlist`, {
          params: {
            id: url
          }
        });
        if (results.data && results.data.success) {
          // eslint-disable-next-line no-console
          // console.log(results.data);
          this.state.playlistInfo = results.data.playlistInfo;
          this.state.items = results.data.items;
        }

        this.state.loading = false;
      } catch (err) {
        this.state.error = err;
        this.state.loading = false;
      }
    },
    async downloadVideo(video) {
      try {
        this.state.downloading = true;
        const results = await axios({
          url: `${this.api_url}/download`,
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

          this.state.downloading = false;
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
