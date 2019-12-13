<template>
  <a class="zf-link" :href="disabled?'javascript:;':href" :zf-type-color="type" :disabled="disabled" v-on="$listeners" >
    <i v-if="icon" :class="icon"></i>
    <slot></slot>
  </a>
</template>

<script>
export default {
  name: "zf-link",
  data() {
    return {};
  },
  props: {
    type: {
      type: String,
      validator(val) {
        return ( ["primary", "success", "info", "warning", "danger"].indexOf(val) !== -1 );
      }
    },
    href: [String, Boolean],
    icon: [String, Boolean],
    disabled: {
      type: Boolean,
      default: false
    }
  }
};
</script>
