<template>
  <div>
    <v-row>
      <v-col cols="12" sm="12" md="6" lg="6" xl="6">
        <audio-player
          :src="track.path"
          ref="audioPlayer"
          @play-started="onPlayStarted"
        />
      </v-col>
      <v-col cols="12" sm="12" md="6" lg="6" xl="6">
        <v-select
          :items="availableLanguages"
          @change="onLanguageSelected"
          outlined
          hide-details
          clearable
        ></v-select>
      </v-col>
    </v-row>
    <v-divider class="my-4" />
  </div>
</template>

<script>
import AudioPlayer from "@/components/player/AudioPlayer";

const Events = {
  PLAY_STARTED_EVENT: "play-started",
  LANGUAGE_SELECTED_EVENT: "language-selected",
  LANGUAGE_UNSELECTED_EVENT: "language-unselected"
};

export default {
  name: "PuzzleElement.vue",
  components: { AudioPlayer },
  props: {
    track: {
      type: Object,
      required: true
    },
    languages: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      currentLanguage: null
    };
  },
  computed: {
    availableLanguages() {
      const languages = [...this.languages];
      if (this.currentLanguage) {
        languages.push(this.currentLanguage);
      }
      return languages.sort();
    }
  },
  methods: {
    onPlayStarted() {
      this.$emit(Events.PLAY_STARTED_EVENT, this.track.id);
    },
    onLanguageSelected(language) {
      if (this.currentLanguage !== null) {
        this.$emit(Events.LANGUAGE_UNSELECTED_EVENT, this.currentLanguage);
      }

      if (language !== null) {
        this.$emit(Events.LANGUAGE_SELECTED_EVENT, language);
      }

      this.currentLanguage = language;
    },
    stopPlayerIfAnotherTrack(trackId) {
      if (trackId !== this.track.id) {
        this.$refs.audioPlayer.stop();
      }
    }
  }
};
</script>

<style scoped></style>
