<template>
    <div>
        <div class="d-flex flex-no-wrap justify-space-between mb-3">
        <h3 class="pt-2 font-weight-light">Queue</h3>
        <v-btn fab small class="ml-auto" color="#d60da0" title="Clear Queue" @click="clearQueue">
            <v-icon>mdi-delete-sweep</v-icon>
        </v-btn>
        </div>
        <v-divider></v-divider>
        <template v-if="queueIds.length > 0">
          <v-card class="my-3" v-for="(item, index) in queueList" :key="index">
              <v-row no-gutters>
                  <v-avatar
                      class="ml-3 mt-3"
                      size="60"
                      tile>
                      <v-img :src=item.thumbnail></v-img>
                  </v-avatar>
                  <div>
                      <v-card-title style="font-size: 0.90vw">{{item.title}}</v-card-title>
                      <v-card-subtitle>{{item.artist}}</v-card-subtitle>
                  </div>
                  <div class="ml-auto">
                      <v-btn fab small class="mx-3 my-3" color="#d60da0" title="Play" @click="playVideo(queueIds[index], queueList[index])">
                          <v-icon>mdi-play</v-icon>
                      </v-btn>
                      <v-btn fab small class="mx-3 my-3" color="#d60da0" title="Delete from Queue" @click="deleteQueue(queueIds[index], queueList[index])">
                          <v-icon>mdi-delete</v-icon>
                      </v-btn>
                  </div>
              </v-row>
          </v-card>
        </template>
    </div>
</template>
<script>
export default {
  props: ['queueIds', 'queueList'],
  methods: {
    playVideo (videoId, videoData) {
      this.$emit('playVideo', videoId)
      this.$emit('videoDetails', videoData)
      const index = this.queueIds.indexOf(videoId)
      if (index > -1) {
        this.queueIds.splice(index, 1)
        this.queueList.splice(index, 1)
      }
    },
    deleteQueue (videoId, videoData) {
      const index = this.queueIds.indexOf(videoId)
      if (index > -1) {
        this.queueIds.splice(index, 1)
        this.queueList.splice(index, 1)
      }
    },
    clearQueue () {
      this.$emit('clearQueue', true)
    }
  }
}
</script>
