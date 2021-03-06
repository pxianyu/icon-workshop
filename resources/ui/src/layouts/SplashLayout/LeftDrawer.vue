<template>
  <q-layout-drawer
    class="splash-layout-left-drawer"
    behavior="desktop"
    :width="160"
    v-model="visible"
  >
    <q-tabs align="justify" class="rd-0">
      <q-tab slot="title" name="preview"
             class="horizontal"
             icon="mdi-tablet-cellphone"
             label="预览" default />

      <q-tab-pane name="preview" class="p-0">
        <VuePerfectScrollbar :settings="{suppressScrollX: true}">
          <q-field label="缩放视图" label-width="12">
            <span class="right-label">
              <q-btn flat round
                     v-if="scale !== 1"
                     @click="scale = 1"
                     size="sm"
                     icon="mdi-restore"></q-btn>
              {{ scale }}倍
            </span>
            <q-slider v-model="scale"
                      :min="0.2" :max="3"
                      :step="0.01"></q-slider>
          </q-field>

          <q-field label="自动缩放" label-width="8" class="field-content-right">
            <q-toggle v-model="autoScale"></q-toggle>
          </q-field>

          <q-field label="屏幕尺寸（px）" class="mt-10" label-width="12">
            <div class="row mt-5">
              <q-input type="number" v-model="width"
                       class="col-5" align="center"
                       min="320" max="5000"></q-input>
              <span class="text-center col-2 pt-3">x</span>
              <q-input type="number" v-model="height"
                       class="col-5" align="center"
                       min="320" max="5000"></q-input>
            </div>
          </q-field>

          <q-btn color="primary"
                 class="full-width mt-10"
                 label="切换横竖屏"
                 @click="swap"
                 icon="mdi-swap-horizontal"></q-btn>

          <q-field label="常用设备" label-width="12" class="mt-15">
            <q-list no-border highlight separator link class="devices">
              <q-item v-for="d in devices" :key="d.name" @click.native="preview(d)">
                <q-item-side :image="d.image"></q-item-side>
                <q-item-main>
                  <q-item-tile label v-html="d.name"></q-item-tile>
                  <q-item-tile sublabel>{{ d.width }} x {{ d.height }}</q-item-tile>
                </q-item-main>
              </q-item>
            </q-list>
          </q-field>
        </VuePerfectScrollbar>
      </q-tab-pane>
    </q-tabs>
  </q-layout-drawer>
</template>

<script>
import { bindState } from '../../common'
import VuePerfectScrollbar from 'vue-perfect-scrollbar'

export default {
  name: 'SplashLayoutLeftDrawer',
  components: {
    VuePerfectScrollbar
  },
  data () {
    return {
      visible: true,
      devices: [
        {
          name: 'iPhone Xs Max/XR',
          image: 'statics/devices/iPhoneXR.png',
          width: 414,
          height: 896
        },
        {
          name: 'iPhone X/Xs',
          image: 'statics/devices/iPhoneX.png',
          width: 375,
          height: 812
        },
        {
          name: 'iPhone 6/7/8 Plus',
          image: 'statics/devices/iPhone8Plus.png',
          width: 414,
          height: 736
        },
        {
          name: 'iPhone 6/7/8',
          image: 'statics/devices/iPhone8.png',
          width: 375,
          height: 667
        },
        {
          name: 'iPhone 5s/SE',
          image: 'statics/devices/iPhoneSE.png',
          width: 320,
          height: 568
        },
        {
          name: 'iPad',
          image: 'statics/devices/iPad.png',
          width: 768,
          height: 1024
        },
        {
          name: '华为P30 pro<br/>OPPO Reno',
          image: 'statics/devices/huaweiP30pro.png',
          width: 1080,
          height: 2340
        },
        {
          name: 'Mate 20 Pro<br/>一加7 Pro',
          image: 'statics/devices/oneplus7Pro.png',
          width: 1440,
          height: 3120
        },
        {
          name: '小米6<br/>一加5',
          image: 'statics/devices/oneplus5.png',
          width: 1080,
          height: 1920
        }
      ]
    }
  },
  computed: {
    ...bindState('Splash', 'scale'),
    ...bindState('Splash', 'autoScale'),
    ...bindState('Splash', 'width'),
    ...bindState('Splash', 'height')
  },
  methods: {
    swap () {
      const a = this.width
      this.width = this.height
      this.height = a
    },
    preview (device) {
      this.width = device.width
      this.height = device.height
    }
  },
  mounted () {
    this.$nextTick(() => {
      this.visible = true
    })
  }
}
</script>

<style lang="scss">
.splash-layout-left-drawer {
  .q-list.devices {
    padding: 0;
    margin-left: -10px;
    margin-right: -10px;

    .q-item {
      padding: 8px 10px 5px;
    }

    .q-item-image {
      width: 40px;
      min-width: 40px;
    }
  }

  .ps-container {
    padding: 10px;
    height: calc(100vh - 52px);
  }

  .q-field {
    .q-if-inner > .relative-position {
      width: 100%;
    }
  }
}
</style>
