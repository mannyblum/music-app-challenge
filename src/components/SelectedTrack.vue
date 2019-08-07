<template>
  <div class="flex flex-wrap">
    <div class="relative w-1/3 ml-auto mr-auto bg-white py-3 px-4 rounded shadow-xl">
      <span class="absolute top-0 right-0 px-1 py-1"
        @click="closePreview"
      >
        <svg class="fill-current h-6 w-6 text-black" role="button" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><title>Close</title><path d="M14.348 14.849a1.2 1.2 0 0 1-1.697 0L10 11.819l-2.651 3.029a1.2 1.2 0 1 1-1.697-1.697l2.758-3.15-2.759-3.152a1.2 1.2 0 1 1 1.697-1.697L10 8.183l2.651-3.031a1.2 1.2 0 1 1 1.697 1.697l-2.758 3.152 2.758 3.15a1.2 1.2 0 0 1 0 1.698z"/></svg>
      </span>
      <img :src="track.artworkUrl100"
        class="ml-auto mr-auto w-40 h-40 border-4 rounded-full border-white shadow-md mb-4"
        :alt="track.collectionName" />
      <div
        class="w-100 h-1 bg-gray-100 mb-4 rounded">
        <div
          :style="'width: '+ progress +'%'"
          class="w-full bg-indigo-500 h-1 rounded"></div>
      </div>
      <button
        class="ml-auto mr-auto block w-100 font-bold rounded border-b-2 border-indigo-400 hover:border-indigo-500 hover:bg-indigo-400 hover:text-white shadow-md py-2 px-6 mb-5 focus:outline-none"
        :class="isPlaying ? 'border-indigo-500 bg-indigo-400 text-white' : 'bg-white text-gray-800'"
        @click.prevent="playPreview">
        {{ isPlaying ? 'Pause' : 'Play' }}
      </button>
      <h5 class="text-center text-gray-900 font-bold text-l mb-2">{{track.trackName}}</h5>
      <p class="text-center text-gray-500 text-sm leading-none">{{track.artistName}}</p>
      <p class="text-center text-gray-700 text-xs mb-10">{{track.collectionName}}</p>
      <p class="absolute bottom-0 left-0 ml-2 mb-2 text-gray-700 text-xs uppercase">
        {{track.releaseDate | formatReleaseDate}} &mdash;
        {{track.primaryGenreName}}
      </p>
      <p class="absolute bottom-0 right-0 mr-2 mb-2 rounded-full w-6 h-6 text-center bg-indigo-400 hover:bg-indigo-600 text-white">
        <a :href="`${track.trackViewUrl}`">$</a>
      </p>
      <audio
        ref="previewAudio"
        @timeupdate="onTimeUpdate"
        @ended="trackEnded"
        :src="track.previewUrl"></audio>
    </div>
  </div>
</template>

<script>
import moment from 'moment';

export default {
  data: function() {
    return {
      isPlaying: false,
      progress: 0
    }
  },
  props: ['track'],
  methods: {
    trackEnded(event) {
      this.isPlaying = false;
    },
    closePreview() {
      var audio = this.$refs.previewAudio;

      audio.pause();

      this.isPlaying = false;
      this.progress = 0;

      this.$emit('previewClosed', false);
    },
    playPreview(event) {
      var audio = this.$refs.previewAudio;

      if (!audio.paused && !audio.ended) {
        this.isPlaying = false;
        audio.pause();
      } else if (audio.paused) {
        this.isPlaying = true;
        audio.play();
      }
    },
    onTimeUpdate() {
      let currentTime;
      let duration;

      if (this.$refs.previewAudio) {
        currentTime = this.$refs.previewAudio.currentTime;
        duration = this.$refs.previewAudio.duration;

        this.progress = (currentTime/duration) * 100;
        console.log('timeupdate', (currentTime/duration) * 100);
      }
    }
  },
  filters: {
    formatReleaseDate(time) {
      return moment(time).format('MM/DD/YYYY');
    }
  }
}
</script>
