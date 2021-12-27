<template>
  <v-app class="app">
    <v-content class="pa-3">
      <v-toolbar>
        <v-toolbar-title>M-P</v-toolbar-title>
      </v-toolbar>
      <v-form>
        <div
          v-for="(track, index) in $options.tracks"
          :key="index"
          class="mt-3"
        >
          <v-row>
            <v-col cols="12" sm="12" md="6" lg="6" xl="6">
              <audio-player
                :src="track"
                ref="audioPlayers"
                @play-started="stopOtherPlayers"
              />
            </v-col>
            <v-col cols="12" sm="12" md="6" lg="6" xl="6">
              <v-select
                :items="$options.languages"
                outlined
                hide-details
              ></v-select>
            </v-col>
          </v-row>
          <v-divider class="my-4" />
        </div>
        <div class="d-flex justify-end">
          <v-btn color="success">Submit</v-btn>
        </div>
      </v-form>
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

const languages = [
  "Belorussian",
  "Bulgarian",
  "Czech",
  "Polish",
  "Russian",
  "Slovak",
  "Slovenian",
  "Ukrainian"
];

export default {
  name: "App",
  components: { AudioPlayer },
  tracks,
  languages,
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
