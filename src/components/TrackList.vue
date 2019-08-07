<template>
<div class="flex mb-4">
  <div class="tracks flex w-11/12 mx-auto p-3">
    <div class="flex flex-wrap pt-2 w-full">
      <div
        class="w-1/2 px-2 mb-4"
        v-for="(track, index) in tracks" :key="index"
        @click="selectTrack(track)"
      >
        <div class="bg-white flex min-h-0 px-4 py-3 rounded-lg hover:shadow-lg cursor-pointer">
          <div class="relative w-full max-w-full h-full flex items-center">
            <span class="absolute text-gray-500 text-xs right-0 top-0 -mt-1 -mr-1 z-50">{{track.trackTimeMillis | formatTrackLength}}</span>
            <img
              class="rounded-full mr-4 w-20 h-20 border-4 border-white shadow-md"
              :src="track.artworkUrl100"
              :alt="track.trackName"
            />
            <div class="relative w-full overflow-x-hidden bg-white rounded-r flex flex-col justify-between leading-normal">
              <h5 class="truncate text-gray-900 font-bold text-l mb-2">{{track.trackName}}</h5>
              <span class="text-gray-500 text-sm mb-0 leading-none">{{track.artistName}}</span>
              <span class="truncate text-gray-700 text-xs">{{track.collectionName}}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  props: [
    'tracks'
  ],
  methods: {
    selectTrack(track) {
      this.$emit('trackSelected', track);
    }
  },
  filters: {
    formatTrackLength(millis) {
      var minutes = Math.floor(millis / 60000);
      var seconds = ((millis % 60000) / 1000).toFixed(0);
      return minutes + ":" + (seconds < 10 ? '0' : '') + seconds;
    }
  }
}
</script>

<style>
.card-columns {
  column-count: 2;
}
</style>
