<template>
    <div>
        <v-text-field
        label="Search for Artist/Song"
        outlined
        dense
        append-icon="mdi-keyboard-return"
        color="#d60da0"
        @keyup.enter="searchList"
        background-color="#1e1e1e">
        </v-text-field>
        <template v-if="ids.length > 0">
          <v-card class="my-3" v-for="(item, index) in list" :key="index">
              <v-row no-gutters>
                  <v-avatar
                      class="ml-3 mt-3"
                      size="60"
                      tile>
                      <v-img :src=item.thumbnail></v-img>
                  </v-avatar>
                  <div>
                      <v-card-title style="font-size: 0.90em">{{item.title}}</v-card-title>
                      <v-card-subtitle>{{item.artist}}</v-card-subtitle>
                  </div>
                  <div class="ml-auto">
                      <v-btn fab small class="mx-3 my-3" color="#d60da0" title="Play" @click="playVideo(ids[index], list[index])">
                          <v-icon>mdi-play</v-icon>
                      </v-btn>
                      <v-btn fab small class="mx-3 my-3" color="#d60da0" title="Add to Queue" @click="addQueue(ids[index], list[index])">
                          <v-icon>mdi-plus</v-icon>
                      </v-btn>
                  </div>
              </v-row>
            </v-card>
          </template>
    </div>
</template>
<script>
import axios from 'axios'

export default {
  data () {
    return {
      list: [{
        id: '',
        title: '',
        artist: '',
        thumbnail: ''
      }],
      ids: []
    }
  },
  methods: {
    searchList (e) {
      this.list = []
      this.ids = []
      axios.get('https://www.googleapis.com/youtube/v3/search?part=snippet&q=' + e.target.value + '&key=API_KEY&type=video')
        .then((response) => {
          for (var item = 0; item < response.data.items.length; item++) {
            this.ids.push(response.data.items[item].id.videoId)
          }
          return axios.get('https://www.googleapis.com/youtube/v3/videos?part=snippet&id=' + this.ids.toString() + '&key=API_KEY')
        })
        .then((response) => {
          for (var item = 0; item < response.data.items.length; item++) {
            var eachItem = {
              id: response.data.items[item].id,
              title: response.data.items[item].snippet.title.split('-')[1],
              artist: response.data.items[item].snippet.channelTitle,
              thumbnail: response.data.items[item].snippet.thumbnails.default.url
            }
            this.list.push(eachItem)
          }
        })
    },
    playVideo (videoId, videoData) {
      this.$emit('playVideo', videoId)
      this.$emit('videoDetails', videoData)
    },
    addQueue (videoId, videoData) {
      this.$emit('addQueueId', videoId)
      this.$emit('addQueueList', videoData)
    }
  }
}
</script>
