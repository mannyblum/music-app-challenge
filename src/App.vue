<template>
<div class="bg-gray-200 h-screen h-full overflow-y-auto">
  <app-header @artistSearch="fetchArtists"></app-header>
  <transition
    enter-active-class="animated fadeIn"
    leave-active-class="animated zoomOut"
    appear
  >
    <app-selected-track
      @previewClosed="closePreview($event)"
      v-if="isTrackSelected"
      :track="track"></app-selected-track>
  </transition>
  <app-track-list :tracks="tracks" @trackSelected="selectTrack"></app-track-list>
</div>
</template>

<script>
import axios from 'axios';

import Header from './components/Header.vue';
import TrackList from './components/TrackList.vue';
import SelectedTrack from './components/SelectedTrack.vue';

export default {
  data () {
    return {
      artist: 'beatles',
      tracks: [],
      selectedTrack: {},
      isTrackSelected: false
    }
  },
  methods: {
    fetchArtists(artist) {
      axios.get(`https://itunes.apple.com/search?term=${artist}&entity=musicTrack&media=music&callback=`)
        .then(response => {
          let items = response.data.results;

          const sortByReleaseDate = items.sort(function(a, b) {
            let itemA = new Date(a.releaseDate);
            let itemB = new Date(b.releaseDate);

            if (itemA < itemB) return -1;
            if (itemB < itemA) return 1;
            return 0;
          });

          this.tracks = sortByReleaseDate;
        })
        .catch(e => {
          console.log('error', e);
        })
    },
    closePreview(event) {
      this.isTrackSelected = false;

    },
    selectTrack(track) {
      console.log('selectTrack', track);
      this.track = track;
      this.isTrackSelected = true;
    },
  },
  components: {
    appHeader: Header,
    appTrackList: TrackList,
    appSelectedTrack: SelectedTrack
  }
}
</script>

<style scoped>
  @import './assets/base.css';
</style>
