<template>
  <div id="item-container">
    <div v-for="item in items.items" :key="item.etag">
      <div class="card" style="width: 24rem;">
        <img :src="item.snippet ? item.snippet.thumbnails ? item.snippet.thumbnails.high.url : null : null" class="card-img-top" alt="...">
        <div class="card-body">
          <h5 class="card-title">{{ item.snippet ? item.snippet.title : '' }}</h5>
          <p class="card-text">
            <a :href="item.snippet ? item.snippet.resourceId ? 'https://youtube.com/watch?v=' + item.snippet.resourceId.videoId : null : null" target="_blank">
              <font-awesome-icon :icon="['fas', 'play']" />
            </a>
          </p>
        </div>
      </div>
    </div> 
  </div>
</template>

<script>
export default {
  name: 'ItemContainer',
  props: {
    items: Object
  },
  data: () => {
    return {
    }
  },
}
</script>

<style scoped>
</style>
