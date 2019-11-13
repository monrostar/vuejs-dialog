<template>
  <el-dialog
    :visible.sync="show"
    width="30%"
    center
    @close="closeAtOutsideClick"
  >
    <template #title v-if="messageHasTitle">-->
      <h6 v-if="options.html" v-html="messageTitle"></h6>
      <h6 v-else>{{ messageTitle }}</h6>
    </template>

    <!-- Start Body container -->
    <section>
      <div v-if="options.html" v-html="messageBody"></div>
      <div v-else>{{ messageBody }}</div>
    </section>
    <!-- End Body container -->

    <template #footer>
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
</template>

<script>
import CancelBtn from '../../components/cancel-btn.vue';
import OkBtn from '../../components/ok-btn.vue';
import DialogMixin from '../../mixins/dialog-mixin';

export default {
  props: {
    show: Boolean,
  },
  data: function() {
    return {};
  },
  mixins: [DialogMixin],
  mounted() {
    this.isHardConfirm && this.$refs.inputElem && this.$refs.inputElem.focus();
  },
  components: {CancelBtn, OkBtn},
};
</script>
