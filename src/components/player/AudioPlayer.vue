<template>
  <audio :src="src"></audio>
</template>
<script>
import Plyr from "plyr";

const controls = [
  "play",
  "restart",
  "progress",
  "current-time",
  "mute",
  "volume"
];

const Events = {
  PLAY_STARTED_EVENT: "play-started"
};

export default {
  name: "AudioPlayer",
  props: {
    src: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      player: {}
    };
  },
  computed: {},
  mounted() {
    this.player = new Plyr(this.$el, {
      controls,
      invertTime: false,
      seekTime: 5
    });

    this.player.on("play", () => {
      this.$emit(Events.PLAY_STARTED_EVENT, this.player);
    });
  },
  beforeUnmount() {
    try {
      this.player.destroy();
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  render() {
    const slots = this.$slots.default;
    return typeof slots === "function" ? slots()[0] : slots;
  },
  methods: {
    stop() {
      this.player.pause();
    }
  }
};
</script>
<style lang="scss">
$second-color: #aaaaaa;

$plyr-audio-controls-background: background-color;
$plyr-audio-control-color: $second-color;

@import "~plyr/src/sass/plyr";
</style>
