<template>
  <div id="item-container" v-if="!loading">
    <b-alert v-if="filteredItems.length > 0 && playlistInfo" variant="dark" show>
      <b-form-input id="search" v-model="search_text" size="md" class="bg-dark text-white playlist-search" placeholder="Search playlist.." />
      <h5 class="alert-heading text-white mb-0">{{ playlistInfo.title }}</h5>
      <p class="text-white">{{ playlistInfo.channelTitle }}</p>
      <p class="text-white mb-0">{{ items.length }} videos</p>
    </b-alert>
    <div v-for="(item, i) in filteredItems" :key="item.videoId + i">
      <b-card v-if="item.title != 'Private video'" :title="item.title" :img-src="item.thumbnails ? item.thumbnails.high.url : null" img-left class="item-card bg-dark text-white border-secondary">
        <b-card-text>
          <a v-b-tooltip.hover title="Play on YouTube" :href="item.videoId ? 'https://youtube.com/watch?v=' + item.videoId : null" target="_blank">
            <font-awesome-icon :icon="['far', 'play-circle']" />
          </a>
          <template v-if="!downloading">
            <a v-b-tooltip.hover title="Download Video" @click="downloadVideo('https://youtube.com/watch?v=' + item.videoId)">
              <font-awesome-icon :icon="['fa', 'download']" />
            </a>
          </template>
          <template v-else>
            <LoadingSpinner :small=true :loading="downloading" />
          </template>
        </b-card-text>
      </b-card>
    </div>
  </div>
  <div v-else class="text-center">
    <LoadingSpinner :loading="loading" />
  </div>
</template>

<script>
import LoadingSpinner from './LoadingSpinner';
import debounce from 'lodash.debounce';

export default {
  name: "ItemContainer",
  components: {
    LoadingSpinner
  },
  props: {
    playlistInfo: Object,
    items: Array,
    loading: Boolean,
    downloading: Boolean
  },
  data: () => {
    return {
      search_text: '',
    };
  },
  computed: {
    filteredItems() {
      return this.items.filter((item) => {
        return item.title.toLowerCase().includes(this.search_text.toLowerCase())
      });
    }
  },
  methods: {
    downloadVideo(video) {
      this.$emit('download-video', video);
    }
  }
};
</script>

<style scoped>
.item-card {
  margin-bottom: 0.5rem !important;
}
.item-card img {
  margin: 10px;
  width: 120px;
  height: 67px;
}
.card-body {
  padding: 0;
  padding-left: 10px;
  padding-top: 10px;
  padding-bottom: 10px;
  text-align: left;
}
.card-body .card-title {
  font-size: 1.2rem;
  font-weight: 500;
  line-height: 2rem;
}
.card-body .card-text a {
  margin-right: 0.7rem;
  color: rgb(180, 180, 180);
}
.card-body .card-text a:hover {
  color: #007bff;
}
div.alert.alert-dark {
  background-color: #3d444a;
}
.playlist-search {
  background-color: #3d444a !important;
  max-width: 350px;
  float: right;
}
</style>
