<template>
    <div class="playground">
        <v-parallax
        src="../assets/banner.png"
        class="playground-frame"
        >
          <div class="lyrics text-center">
            <span class="lyrics-style py-3">{{lyrics}}</span>
          </div>
        </v-parallax>
        <iframe v-if="videoId" id="ytplayer" type="text/html" width="320" height="240"
        :src="'https://www.youtube.com/embed/'+videoId+'?autoplay=1&rel=0&fs=0&widget_referrer=https://tunify.netlify.app/'"
        frameborder="0" allow="autoplay"></iframe>
    </div>
</template>
<style scoped>
    .playground {
        max-height: 700px;
        display: block;
        position: -webkit-sticky; /* Safari */
        position: sticky;
        top: 94px;
    }
    .playground-frame {
      border-radius: 4px;
    }
    /* Hide scrollbar for Chrome, Safari and Opera */
    .lyrics::-webkit-scrollbar {
        display: none;
    }
    /* Hide scrollbar for IE and Edge */
    .lyrics {
        -ms-overflow-style: none;
        position: absolute;
        overflow-y: auto;
        white-space: pre;
        height: 260px;
        top: 4px;
        left: 0;
        right: 0;
        margin: auto;
        z-index: 3;
    }
    .lyrics-style {
      width: 100%;
      font-size: 1.25vw;
    }
    #ytplayer {
      border-radius: 4px;
      position: absolute;
      bottom: 3px;
      left: 0;
      right: 0;
      margin: auto;
    }
</style>
<script>
import axios from 'axios'
const config = {
  headers: {
    'x-rapidapi-host': 'canarado-lyrics.p.rapidapi.com',
    'x-rapidapi-key': 'APIKEY',
    'Access-Control-Allow-Origin': 'https://tunify.netlify.app'
  }
}

export default {
  props: ['videoId', 'videoData'],
  data () {
    return {
      lyrics: 'Welcome to Tunify',
      pause: true
    }
  },
  watch: {
    videoData: function () {
      let fetchedLyrics = null
      var newTitle = this.videoData.title
      // remove everything after (
      if (this.videoData.title.includes('(')) {
        newTitle = this.videoData.title.split(' (')[0]
      } else if (this.videoData.title.includes('[')) {
        newTitle = this.videoData.title.split(' [')[0]
      } else if (this.videoData.title.includes('|')) {
        newTitle = this.videoData.title.split(' |')[0]
      }
      newTitle = newTitle.trim()
      var url = 'https://canarado-lyrics.p.rapidapi.com/lyrics/' + newTitle
      axios.get(url, config)
        .then((response) => {
          for (var i = 0; i < response.data.content.length; i++) {
            if (response.data.content[i].artist.includes(this.videoData.artist)) {
              fetchedLyrics = response.data.content[i].lyrics
              break
            }
          }
          if (!fetchedLyrics) {
            if (response.data.content[0].title.includes(newTitle)) {
              fetchedLyrics = response.data.content[0].lyrics
            } else {
              fetchedLyrics = this.videoData.title + ' - ' + this.videoData.artist
            }
          }
          this.lyrics = fetchedLyrics
        })
        .catch((error) => {
          console.log(error)
        })
    }
  }
}
</script>
