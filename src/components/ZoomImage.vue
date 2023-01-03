<template lang="pug">
img( :src="src" :data-zoom="_srcZoom", width="100%" height="100%")
</template>

<script>
import Drift from 'drift-zoom';

export default {
  props: {
    src: {
      type: String,
      required: true,
    },
    srcZoom: String,
    options: {
      type: Object,
      default: () => ({
        zoomFactor: 1,
      }),
    },
  },
  data: () => ({ drift: null }),
  computed: {
    _srcZoom() {
      return this.srcZoom || this.src;
    },
  },
  methods: {
    setDrift(opts) {
      if (document) {
        const _opts = {};

        if (opts.paneContainer) {
          _opts.paneContainer = document.querySelector(opts.paneContainer);
        }
        if (opts.inlineContainer) {
          _opts.inlineContainer = document.querySelector(opts.inlineContainer);
        }
        if (opts.boundingBoxContainer) {
          _opts.boundingBoxContainer = document.querySelector(
            opts.boundingBoxContainer
          );
        }

        _opts.onShow = () => this.$emit('show');
        _opts.onHide = () => this.$emit('hide');

        this.drift = new Drift(this.$el, Object.assign({}, opts, _opts));
      }
    },
  },
  mounted() {
    this.setDrift(this.options);
  },
  destroyed() {
    this.drift && this.drift.destroy();
  },
  watch: {
    options(newValue, oldValue) {
      if (JSON.stringify(newValue) !== JSON.stringify(oldValue)) {
        this.drift && this.drift.destroy();
        this.setDrift(newValue);
      }
    },
    srcZoom(newValue, oldValue) {
      if (newValue && newValue !== oldValue) {
        this.drift && this.drift.setZoomImageURL(newValue);
      }
    },
  },
};
</script>

<style scoped>
img {
  object-fit: contain;
}
</style>
