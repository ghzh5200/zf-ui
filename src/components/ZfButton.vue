<template>
  <button class="zf-button" :zf-type-bg="type" :type="nativeType" :zf-size="size" :zf-diffuse="diffusion" v-on="listeners" :zf-round="round" :zf-loading="loading" :zf-smooth="smooth">
    <i v-if="loading" class="zf-icon-loading"></i>
    <i v-if="icon" :class="icon"></i>
    <i v-if="diffusion" class="zf-diffuse"></i>
    <slot v-if="!round"></slot>
  </button>
</template>

<script>
export default {
  name: "zf-button",
  data() {
    return {
      tiemout: false,
      diffusion: false
    };
  },
  props: {
    size: {
      type: [String, Boolean],
      default: false
    },
    type: {
      type: [String, Boolean],
      default: false
    },
    icon: String,
    diffuse: {
      type: Boolean,
      default: false
    },
    round: {
      type: Boolean,
      default: false
    },
    smooth: {
      type: Boolean,
      default: false
    },
    nativeType: String,
    loading: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    click() {
      if (!this.diffuse || this.tiemout) return;
      this.diffusion = true;
      this.tiemout = setTimeout(() => {
        this.diffusion = false;
        this.tiemout = false;
      }, 1000);
    }
  },
  computed: {
    listeners() {
      return Object.assign(
        {},
        this.$listeners,
        {
          click: event => {
            this.click();
            this.$emit('click',event);
          }
        }
      );
    },
  }
};
</script>
