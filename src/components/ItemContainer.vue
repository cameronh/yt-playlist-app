<template>
  <div id="item-container" v-if="!loading">
    <b-alert v-if="showPlaylistInfo" variant="dark" show>
      <InputForm @input-changed="search_text = $event" size="md" classes="text-white playlist-search" placeholder="Search playlist.." />
      <h5 class="alert-heading text-white mb-0">{{ playlistInfo.title }}</h5>
      <p class="text-white">{{ playlistInfo.channelTitle }}</p>
      <p class="text-white mb-0">{{ items.length }} videos</p>
    </b-alert>
    <div v-for="(item, i) in filteredItems" :key="item.videoId + i">
      <b-card v-if="item.title != 'Private video'" :title="item.title" :img-src="item.thumbnails ? item.thumbnails.high.url : null" img-left class="item-card bg-dark text-white border-secondary">
        <b-card-text>
          <a v-b-tooltip.hover title="Play on YouTube" :href="item.videoId ? youtube_watch_prefix + item.videoId : null" target="_blank">
            <font-awesome-icon :icon="['far', 'play-circle']" />
          </a>
          <template v-if="!downloading">
            <a v-b-tooltip.hover title="Download Video" @click="downloadVideo(youtube_watch_prefix + item.videoId)">
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
import InputForm from './InputForm';
import debounce from 'lodash.debounce';

export default {
  name: "ItemContainer",
  components: {
    LoadingSpinner,
    InputForm
  },
  props: {
    playlistInfo: Object,
    items: Array,
    loading: Boolean,
    downloading: Boolean
  },
  data: () => {
    return {
      youtube_watch_prefix: 'https://youtube.com/watch?v=',
      search_text: '',
      debounced_search_text: '',
    };
  },
  watch: {
    'search_text': debounce(function(newVal, oldVal) {
      this.debounced_search_text = '';
      
      if (newVal !== oldVal) {
        this.debounced_search_text = newVal;
      }
    }, 500)
  },
  computed: {
    showPlaylistInfo() {
      return this.playlistInfo.title || this.search_text.length > 0;
    },
    filteredItems() {
      return this.items.filter((item) => {
        return item.title.toLowerCase().includes(this.debounced_search_text.toLowerCase())
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
</style>
