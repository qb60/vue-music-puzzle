<template>
  <v-app class="app">
    <v-content>
      <v-toolbar>
        <v-app-bar-nav-icon></v-app-bar-nav-icon>
        <v-toolbar-title>M-P</v-toolbar-title>
        <v-spacer></v-spacer>
      </v-toolbar>
      <div>
        <audio-player
          v-for="(track, index) in $options.tracks"
          :src="track"
          :key="index"
          ref="audioPlayers"
          @play-started="stopOtherPlayers"
        />
      </div>
    </v-content>
  </v-app>
</template>

<script>
import AudioPlayer from "./components/player/AudioPlayer.vue";

const tracks = [
  "tracks/1.mp3",
  "tracks/2.mp3",
  "tracks/3.mp3",
  "tracks/4.mp3",
  "tracks/5.mp3",
  "tracks/6.mp3",
  "tracks/7.mp3",
  "tracks/8.mp3"
];

export default {
  name: "App",
  components: { AudioPlayer },
  tracks,
  methods: {
    stopOtherPlayers(player) {
      this.$refs.audioPlayers.forEach((audioPlayer) =>
        audioPlayer.stopIfNotYou(player)
      );
    }
  }
};
</script>

<style lang="scss">
$back: #333333;

.app {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  min-height: 100vh;
  // background-color: $back;
}
</style>
