<template>
  <div :class="options.customClass">
    <transition :name="animation" @after-leave="animationEnded('content')" appear>
      <el-dialog
        :visible.sync="show"
        width="30%"
        center
        @close="closeAtOutsideClick">

        <template v-if="messageHasTitle" slot="title">
          <h6 v-if="options.html" v-html="messageTitle"></h6>
          <h6 v-else>{{ messageTitle }}</h6>
        </template>

        <!-- Start Body container -->
        <div>
          <div v-if="options.html" v-html="messageBody"></div>
          <div v-else>{{ messageBody }}</div>
        </div>
        <!-- End Body container -->

        <template slot="footer">
          <component @click="clickLeftBtn()" :is="leftBtnComponent" :loading="loading"
            :enabled="leftBtnEnabled" :options="options" :focus="leftBtnFocus">
            <span v-if="options.html" v-html="leftBtnText"></span>
            <span v-else="">{{ leftBtnText }}</span>
          </component>

          <component :is="rightBtnComponent" @click="clickRightBtn()" :loading="loading"
            :enabled="rightBtnEnabled" :options="options" :focus="rightBtnFocus">
            <span v-if="options.html" v-html="rightBtnText"></span>
            <span v-else="">{{ rightBtnText }}</span>
          </component>
        </template>

      </el-dialog>
    </transition>
  </div>
</template>

<script>
import CancelBtn from '../components/cancel-btn.vue';
import OkBtn from '../components/ok-btn.vue';
import {ANIMATION_TYPES, CONFIRM_TYPES, DIALOG_TYPES} from '../constants';
import DialogMixin from '../mixins/dialog-mixin';

export default {
  mixins: [DialogMixin],
  components: {CancelBtn, OkBtn},
  data: function() {
    return {
      show: true,
      closed: false,
      endedAnimations: [],
    };
  },
  props: {
    options: {
      type: Object,
      required: true,
    },
    escapeKeyClose: {
      type: Boolean,
      'default': false,
    },
    registeredViews: {
      type: Object,
      'default': function() {
        return {};
      },
    },
  },
  watch: {
    'escapeKeyClose': function(val) {
      if (val === true) {
        this.cancelBtnDisabled ? this.mix_dialog_proceed() : this.cancel();
      }
    },
  },
  computed: {
    animation() {
      let a = this.options.animation.toUpperCase();
      return ANIMATION_TYPES.hasOwnProperty(a)
        ? ANIMATION_TYPES[a]
        : ANIMATION_TYPES.ZOOM;
    },
    loaderEnabled() {
      return !!this.options.loader;
    },
    isHardConfirm() {
      return this.options.window === DIALOG_TYPES.CONFIRM &&
        this.options.type === CONFIRM_TYPES.HARD;
    },
    isPrompt() {
      return (this.options.window === DIALOG_TYPES.PROMPT);
    },
  },
  methods: {
    closeAtOutsideClick() {
      if (this.options.backdropClose === true) {
        this.cancelBtnDisabled ? this.mix_dialog_proceed() : this.cancel();
      }
    },
    cancel() {
      if (this.loading === true) {
        return;
      }
      this.close();
    },
    close() {
      this.show = false;
      this.closed = true;
      this.mix_dialog_close();
    },
    animationEnded(type) {
      this.endedAnimations.push(type);

      if (this.endedAnimations.indexOf('backdrop') !== -1
        && this.endedAnimations.indexOf('content') !== -1
      ) {
        this.options.promiseRejecter(false);
        this.mix_dialog_close();
      }
    },
  },
  beforeDestroy() {
    if (this.closed === false) {
      this.cancelBtnDisabled ? this.mix_dialog_proceed() : this.cancel();
    }
  },
};
</script>
