<template>
  <div style="padding-bottom: 25px">

    <header>
      <a href="?lang=en" class="lang-btn">English</a>
      <a href="?lang=zh" class="lang-btn">中文</a>

      <div class="header-cont centered">
        <h1 class="title">Vuejs Dialog Plugin</h1>
        <p class="description">A lightweight, promise based alert, prompt and confirm dialog</p>
      </div>
    </header>

    <div class="container">

      <aside v-if="0">
        <div class="sidebar">
          <ul class="list-unstyled nav">
            <li v-for="_ in 8"><a class="button" href="#">Nav item</a></li>
          </ul>
        </div>
      </aside>

      <main>
        <section>
          <h2 class="title-case">{{ trans('content.titles.method_usage')}}</h2>
          <hr/>
          <h4>
            <button class="button" @click="showAlertDialog()">{{ trans('content.examples.method_usage.1')
              }}
            </button>
          </h4>

          <h4>
            <button class="button" @click="showPromptDialog()">{{ trans('content.examples.method_usage.9')
              }}
            </button>
          </h4>

          <h4>
            <button class="button" @click="showHtmlDialog()">{{ trans('content.examples.method_usage.2')
              }}
            </button>
          </h4>

          <h4>
            <button class="button" @click="showBasicDialog()">{{ trans('content.examples.method_usage.3')
              }}
            </button>
          </h4>

          <h4>
            <button class="button" @click="showLoadingDialog()">{{ trans('content.examples.method_usage.4')
              }}
            </button>
          </h4>

          <h4>
            <button class="button" @click="showReversedDialog()">{{ trans('content.examples.method_usage.5')
              }}
            </button>
          </h4>

          <h4>
            <button class="button" @click="showAnimationFadeDialog()">
              {{ trans('content.examples.method_usage.6') }}
            </button>
          </h4>

          <h4>
            <button class="button" @click="showAnimationBounceDialog()">
              {{ trans('content.examples.method_usage.7') }}
            </button>
          </h4>
          <h4>
            <button class="button" @click="showBasicDialogCloseClickOutside()">
              {{ trans('content.examples.method_usage.8') }}
            </button>
          </h4>
        </section>

        <section>
          <h2 class="title-case">{{ trans('content.titles.directive_usage')}}</h2>
          <hr/>

          <h4>
            <button class="button" v-confirm="trans('messages.directive_string')">
              {{ trans('content.examples.directive_usage.1')
              }} v-confirm="'{{ trans('messages.directive_string') }}'"
            </button>
          </h4>

          <h4>
            <a href="http://example.com" v-confirm="trans('messages.directive_link')">
              {{ trans('content.examples.directive_usage.2') }}
            </a>
          </h4>

          <h4>
            <button class="button" v-confirm="{
                        loader: true,
                        message: trans('messages.directive_object'),
                        ok: clickOkHandler,
                        cancel: clickCancelHandler}"
            >
              {{ trans('content.examples.directive_usage.3') }}
            </button>
          </h4>

          <form @submit.prevent="submitDemo1Form()" style="display: inline-block;">
            <fieldset style="padding: 10px;">
              <input v-model="forms.demo1.name" type="text" name="name" class="input-elem"
                :placeholder="trans('placeholders.your_name')" style="margin-bottom: 10px;"/>

              <button class="button" type="reset" v-confirm="trans('messages.form_reset')">
                {{ trans('content.words.reset') }}
              </button>
              <button class="button" type="submit" v-confirm="trans('messages.form_submit')">
                {{ trans('content.words.submit') }}
              </button>
            </fieldset>
          </form>
        </section>

        <section>
          <h2>{{ trans('content.titles.confirmation_types')}}</h2>
          <hr/>

          <p>{{ trans('content.descriptions.confirmation_types.1')}}</p>
          <p>{{ trans('content.descriptions.confirmation_types.2')}}</p>

          <h4>
            <button class="button" @click="showSoftConfirmDialog()">
              {{ trans('content.examples.confirmation_types.1') }}
            </button>
          </h4>

          <h4>
            <button class="button" @click="showHardConfirmDialog()">
              {{ trans('content.examples.confirmation_types.2') }}
            </button>
          </h4>
        </section>

        <section>
          <h2>Extending the dialog</h2>
          <hr/>

          <h4>
            <button class="button" @click="showDialogWithCustomView()">Custom View/Component</button>
          </h4>

          <iframe width="100%" height="600"
            src="https://jsfiddle.net/godofbrowser/pktL3mjb/embedded/js,html,css,result/dark/"
            allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>
        </section>
      </main>

    </div>

    <footer v-if="0"><p>&copy; 2018 - footer text content here</p></footer>


    <notifications position="bottom left"></notifications>

    <a href="https://github.com/godofbrowser/vuejs-dialog"
      v-confirm:soft="{html: true, message: exitMessage, cancelText: `Stay on Page`, okText: trans('content.words.source_code')}">
      <img style="position: absolute; top: 0; right: 0; border: 0;"
        src="https://camo.githubusercontent.com/e7bbb0521b397edbd5fe43e7f760759336b5e05f/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f677265656e5f3030373230302e706e67"
        alt="Fork me on GitHub"
        data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_green_007200.png">
    </a>
  </div>
</template>

<script>
import trans from '../js/translations';
import {popupWindow} from '../js/util';
import TestView from './custom-component.vue';

const DIALOG_TEST_VIEW = 'test';

const exitMessage = `\
<p style="text-align: center; margin: 0;">
    <span class="dg-highlight-1">Thank You!</span>
     <br/><br/>
     I hope you find it useful
</p>`;

export default {
  data() {
    return {
      exitMessage,
      forms: {
        demo1: {
          name: null,
        },
      },
    };
  },
  mounted() {
    console.log('mounted app');

    this.$dialog.registerComponent(DIALOG_TEST_VIEW, TestView);
  },
  methods: {
    trans,
    submitDemo1Form() {
      let msg = trans('placeholders.your_name') + ': ';
      msg += this.forms.demo1.name || trans('messages.empty_name');
      this.$notify(msg);
    },
    showAlertDialog() {
      this.$dialog.alert(trans('messages.alert'));
    },
    showPromptDialog() {
      this.$dialog.prompt({title: 'Let\'s hear from you', body: trans('messages.prompt'), loader: true}).
           then((dialog) => {
             this.$dialog.alert(dialog.data || '[empty]');
           });
    },
    showBasicDialog() {
      this.$dialog.confirm(trans('messages.basic')).then(() => {
        this.$notify({type: 'success', text: trans('messages.click_continue')});
      }).catch(() => {
        this.$notify({type: 'success', text: trans('messages.click_cancel')});
      });
    },
    showBasicDialogCloseClickOutside() {
      this.$dialog.confirm(trans('messages.basic'), {
        backdropClose: true,
      }).then(() => {
        this.$notify({type: 'success', text: trans('messages.click_continue')});
      }).catch(() => {
        this.$notify({type: 'success', text: trans('messages.click_cancel')});
      });
    },

    showSoftConfirmDialog() {
      this.$dialog.confirm(trans('messages.soft'), {type: 'soft'}).then(() => {
        this.$notify({type: 'success', text: trans('messages.click_continue')});
      }).catch(() => {
        this.$notify({type: 'success', text: trans('messages.click_cancel')});
      });
    },
    showHardConfirmDialog() {
      this.$dialog.confirm(trans('messages.hard'), {type: 'hard', html: true, verification: 'let me go'}).then(() => {
        this.$notify({type: 'success', text: trans('messages.click_continue')});
      }).catch(() => {
        this.$notify({type: 'success', text: trans('messages.click_cancel')});
      });
    },
    showHtmlDialog() {
      this.$dialog.alert(trans('messages.html'), {html: true, okText: trans('content.words.dismiss')});
    },
    showAnimationBounceDialog() {
      this.$dialog.alert(trans('messages.html'), {html: true, animation: 'bounce'});
    },
    showAnimationFadeDialog() {
      this.$dialog.alert(trans('messages.html'), {html: true, animation: 'fade'});
    },

    showDialogWithCustomView() {
      this.$dialog.confirm({
        title: 'Show some love!',
        body: 'Kindly share the plugin if you consider it useful',
      }, {
        view: DIALOG_TEST_VIEW,
        html: true,
        animation: 'fade',
        backdropClose: true,
      }).then(dialog => {
        console.log(dialog.data);

        const ENCODED_TITLE = encodeURIComponent('A dialog plugin for VueJs');
        const ENCODED_URL = encodeURIComponent('https://github.com/godofbrowser/vuejs-dialog');

        let links = {
          facebook: `https://www.facebook.com/sharer/sharer.php?u=${ ENCODED_URL }&t=${ ENCODED_TITLE }`,
          twitter: `https://twitter.com/share?url=${ ENCODED_URL }&text=${ ENCODED_TITLE }`,
          googleplus: `https://plus.google.com/share?url=${ ENCODED_URL }`,
          linkedin: `https://www.linkedin.com/shareArticle?mini=true&url=${ ENCODED_URL }&title=${ ENCODED_TITLE }`,
        };

        const network = dialog.data;

        if (links[network]) {
          popupWindow(links[network], network);
        }
      });
    },

    showLoadingDialog() {
      this.$dialog.confirm(trans('messages.loading'), {
        html: true,
        okText: trans('content.words.proceed'),
        loader: true,
      }).then((dialog) => {
        setTimeout(() => {

          this.$notify({type: 'success', text: trans('messages.loading_completed')});
          dialog.close();
        }, 2500);

      }).catch(() => {
        this.$notify({text: trans('messages.loading_canceled')});
      });
    },

    showReversedDialog() {
      this.$dialog.confirm(trans('messages.reverse'), {
        html: true,
        okText: trans('content.words.proceed'),
        loader: true,
        reverse: true,
      }).then((dialog) => {
        setTimeout(() => {

          this.$notify({type: 'success', text: trans('messages.loading_completed')});
          dialog.close();
        }, 2500);

      }).catch(() => {
        this.$notify({text: trans('messages.loading_canceled')});
      });
    },
    clickOkHandler(dialog) {
      console.log('Dialog: ', dialog);
      dialog.loading && dialog.close();
    },
    clickCancelHandler() {
      this.$notify({type: 'error', text: trans('messages.click_cancel')});
    },
  },
};
</script>

<style lang="scss">
  @import "../scss/app";

  .vue-notification {
    padding: 10px;
    margin: 15px;

    font-size: 18px;
    font-family: "Noto Sans", sans-serif;

    color: #ffffff;
    background: #44A4FC;
    border-left: 5px solid #187FE7;
  }
</style>
