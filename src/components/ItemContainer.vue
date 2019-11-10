<template>
  <div id="item-container" v-if="!loading">
    <div v-for="(item, i) in items.items" :key="item.videoId + i">
      <b-card v-if="item.title != 'Private video'" :title="item.title" :img-src="item.thumbnails ? item.thumbnails.high.url : null" img-left class="item-card bg-dark text-white border-secondary">
        <b-card-text>
          <a v-b-tooltip.hover title="Copy link to clipboard" href="#" target="_blank">
            <font-awesome-icon :icon="['far', 'copy']" />
          </a>
          <a v-b-tooltip.hover title="Play on YouTube" :href="item.videoId ? 'https://youtube.com/watch?v=' + item.videoId : null" target="_blank">
            <font-awesome-icon :icon="['far', 'play-circle']" />
          </a>
          <a v-b-tooltip.hover title="Download Video" href="#">
            <font-awesome-icon :icon="['fa', 'download']" />
          </a>
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

export default {
  name: "ItemContainer",
  components: {
    LoadingSpinner
  },
  props: {
    items: Object,
    loading: Boolean
  },
  data: () => {
    return {};
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
</style>
