<template>
  <el-button v-if="enabled" type="primary"
    :loading="loading"
    @click.prevent="proceed()" ref="btn" :disabled="is_disabled" :autofocus="focus">
        <span>
            <slot></slot>
            <span v-if="soft_confirm">({{ clicks_remaining }})</span>
        </span>
  </el-button>
</template>

<script>
import {CONFIRM_TYPES} from '../constants';

export default {
  data() {
    return {
      clicks_count: 0,
    };
  },
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
  computed: {
    soft_confirm() {
      return (this.options.type === CONFIRM_TYPES.SOFT);
    },
    hard_confirm() {
      return (this.options.type === CONFIRM_TYPES.HARD);
    },
    is_disabled() {
      return (this.$parent.okBtnDisabled);
    },
    clicks_remaining() {
      return Math.max((this.options.clicksCount - this.clicks_count), 0);
    },
  },
  methods: {
    proceed() {
      if (!this.is_disabled && this.validateProceed()) {
        this.$emit('click');
      }
    },
    validateProceed() {
      switch (this.options.type) {
      case CONFIRM_TYPES.SOFT:
        this.clicks_count++;
        return (this.clicks_count >= this.options.clicksCount);
        break;
      case CONFIRM_TYPES.BASIC:
      default:
        return true;
      }
    },
  },
};
</script>

