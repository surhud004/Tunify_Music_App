<template>
  <v-app>
    <v-app-bar app>
      <v-img
        class="mx-2"
        src="./assets/logo.png"
        max-height="60"
        max-width="60"
        contain
      ></v-img>
      <v-toolbar-title class="title-style ml-2">
        Tunify
      </v-toolbar-title>
    </v-app-bar>
    <v-container fluid>
      <v-content>
        <v-row>
          <v-col lg="3" md="12" sm="12">
            <List @playVideo="videoId = $event" @videoDetails="videoData = $event"
            @addQueueId="queueIds.push($event)" @addQueueList="queueList.push($event)"/>
          </v-col>
          <v-col lg="6" md="12" sm="12">
            <Playground :videoId="videoId" :videoData="videoData"/>
          </v-col>
          <v-col lg="3" md="12" sm="12">
            <Queue :queueIds="queueIds" :queueList="queueList"
            @playVideo="videoId = $event" @videoDetails="videoData = $event"
            @clearQueue="clearQueue"/>
          </v-col>
        </v-row>
      </v-content>
    </v-container>
    <v-footer class="justify-center" app>
      Designed and Coded by : surhud004 &copy; 2020 <!-- Build Date : 16th June 2020 -->
    </v-footer>
  </v-app>
</template>
<style scoped>
#app {
  font-family: 'Montserrat', sans-serif;
}
.title-style {
  font-weight: bold;
  letter-spacing: 0.15em;
  color: #d60da0;
}
</style>
<script>
import List from './components/List'
import Playground from './components/Playground'
import Queue from './components/Queue'

export default {
  name: 'App',

  components: {
    List,
    Playground,
    Queue
  },

  data () {
    return {
      videoId: '',
      videoData: '',
      queueList: [],
      queueIds: []
    }
  },
  methods: {
    clearQueue () {
      this.queueIds = []
      this.queueList = []
    }
  }
}
</script>
