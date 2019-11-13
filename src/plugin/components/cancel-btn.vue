<template>
  <el-button v-if="enabled" type="danger"
    @click.prevent="$emit('click')" ref="btn" :autofocus="focus">
    <slot></slot>
  </el-button>
</template>

<script>
export default {
  props: {
    enabled: {
      required: false,
      type: Boolean,
      'default': true,
    },
    options: {
      required: true,
      type: Object,
    },
    focus: {
      required: false,
      type: Boolean,
      'default': false,
    },
    loading: {
      required: false,
      type: Boolean,
      'default': false,
    },
  },
};
</script>
