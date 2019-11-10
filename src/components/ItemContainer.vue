<template>
  <div id="item-container" v-if="!loading">
    <div v-for="(item, i) in items.items" :key="item.videoId + i">
      <b-card v-if="item.title != 'Private video'" :title="item.title" :img-src="item.thumbnails ? item.thumbnails.high.url : null" img-top style="max-width: 20rem;" class="mb-2">
        <b-card-text>
          <a v-b-tooltip.hover title="Play on YouTube" :href="item.videoId ? 'https://youtube.com/watch?v=' + item.videoId : null" target="_blank">
            <font-awesome-icon :icon="['fas', 'play']" />
          </a>
        </b-card-text>
      </b-card>
    </div>
  </div>
  <div v-else>
    <LoadingSpinner loading="loading" />
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
</style>
