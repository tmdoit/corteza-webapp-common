<template>
  <div class="inline">
    <img
      ref="image"
      :key="src"
      :src="src"
      :title="title"
      :alt="alt"
      :class="getClass"
      :style="previewStyle"
      :width="getWidth"
      :height="getHeight"
      @click="$emit('openPreview', {})"
      @error.once="reloadBrokenImage"
      @load="loaded=true"
    >
  </div>
</template>

<script>
import Base from '../../common/Base'

export default {
  extends: Base,

  props: {
    alt: {
      required: false,
      type: String,
      default: null,
    },
    title: {
      required: false,
      type: String,
      default: null,
    },
  },

  data () {
    return {
      loaded: false,
    }
  },

  computed: {
    getClass () {
      const rtr = [...this.previewClass]
      if (this.$listeners.click) {
        rtr.push('clickable')
      }
      if (this.loaded) {
        rtr.push('loaded')
      }
      return rtr
    },

    getWidth () {
      return this.meta.preview.image.width
    },
    getHeight () {
      return this.meta.preview.image.height
    },
  },

  methods: {
    reloadBrokenImage (ev) {
      if (ev.target && ev.target.src) {
        window.setTimeout(() => {
          // This forces Vue to re-try image download
          // eslint-disable-next-line
          ev.target.src = ev.target.src
        }, 500)
      }
    },
  },
}
</script>

<style scoped lang="scss">
div {
  margin: 0 auto;
  object-fit: contain;
  img {
    &.loaded {
      width: auto;
      height: auto;
      margin: 0 auto;
      display: block;
    }
  }
  &.inline {
    img {
      cursor: zoom-in;
    }
  }
}
</style>
