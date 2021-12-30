<template>
  <v-app class="app">
    <v-main class="pa-4">
      <v-toolbar>
        <v-toolbar-title>M-P</v-toolbar-title>
      </v-toolbar>
      <v-form class="mt-4">
        <puzzle-element
          v-for="track in $options.tracks"
          :key="track.id"
          :track="track"
          :languages="availableLanguages"
          ref="puzzleElements"
          @play-started="stopOtherPlayers"
          @language-selected="onLanguageSelected"
          @language-unselected="onLanguageUnselected"
        />
        <div class="d-flex justify-end">
          <v-btn color="success" :disabled="!isFromSubmittable">Submit</v-btn>
        </div>
      </v-form>
    </v-main>
  </v-app>
</template>

<script>
import PuzzleElement from "./components/PuzzleElement";

const tracks = [
  { id: 0, path: "tracks/1.mp3" },
  { id: 1, path: "tracks/2.mp3" },
  { id: 2, path: "tracks/3.mp3" },
  { id: 3, path: "tracks/4.mp3" },
  { id: 4, path: "tracks/5.mp3" },
  { id: 5, path: "tracks/6.mp3" },
  { id: 6, path: "tracks/7.mp3" },
  { id: 7, path: "tracks/8.mp3" }
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
  components: { PuzzleElement },
  tracks,
  languages,
  data() {
    return {
      availableLanguages: []
    };
  },
  beforeMount() {
    this.availableLanguages.push(...languages);
  },
  computed: {
    isFromSubmittable() {
      return this.availableLanguages.length === 0;
    }
  },
  methods: {
    stopOtherPlayers(trackId) {
      this.$refs.puzzleElements.forEach((puzzleElement) =>
        puzzleElement.stopPlayerIfAnotherTrack(trackId)
      );
    },
    onLanguageSelected(language) {
      this.availableLanguages = this.availableLanguages.filter(
        (lang) => lang !== language
      );
    },
    onLanguageUnselected(language) {
      this.availableLanguages = [...this.availableLanguages, language];
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
