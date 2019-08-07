<template>
  <div class="flex mb-4">
    <div class="w-11/12 mx-auto mt-10 p-3">
      <div class="flex bg-white border-0 rounded-lg items-center py-2 px-2 shadow-lg">
        <input
          class="appearance-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none"
          type="text"
          v-model="artist"
          @keyup.enter="searchArtist"
          placeholder="Search Artist"
          aria-label="Search Artist"
          aria-describedby="search-button"
           />
        <button
          class="h-12 flex-shrink-0 bg-indigo-400 hover:bg-indigo-500 border-indigo-400 hover:border-indigo-500 text-sm border-4 text-white py-1 px-2 rounded"
          @click="searchArtist"
          id="search-button"
          type="button">
          Search
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      artist: 'The Beatles'
    }
  },
  methods: {
    searchArtist() {
      this.$emit('artistSearch', this.artist);
    }

  }
}
</script>

