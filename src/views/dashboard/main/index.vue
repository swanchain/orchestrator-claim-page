<template>
  <section id="container-main" class="flex-row">
    <div class="module lang-max">
      <h1 class="font-53 weight-7 font-black">CONGRATULATIONS!</h1>
      <h2 class="font-52 weight-4 font-bold">Swan Jupiter Testnet Winners</h2>
      <p class="font-41 weight-3">Click below to claim your reward.</p>
      <el-button class="font-29 font-bold" color="#447dff" round @click="claimShow=true">CLAIM</el-button>
    </div>

    <ul class="media flex-row">
      <li v-for="m in mediaData" :key="m">
        <img :src="m.img" @click="system.$commonFun.goLink(m.link)" />
      </li>
    </ul>

    <pop-ups v-if="claimShow" :claimShow="claimShow" @hardClose="hardClose"></pop-ups>
  </section>
</template>

<script>
import { defineComponent, computed, onMounted, watch, ref, reactive, getCurrentInstance } from 'vue'
import popUps from "@/components/popups"
import { useStore } from "vuex"
import { useRouter, useRoute } from 'vue-router'
import {
  CircleCheck
} from '@element-plus/icons-vue'
import { ElButton } from "element-plus"
export default defineComponent({
  components: {
    CircleCheck, popUps, ElButton
  },
  setup () {
    const store = useStore()
    const bodyWidth = ref(document.body.clientWidth <= 768 ? 30 : 50)
    const system = getCurrentInstance().appContext.config.globalProperties
    const route = useRoute()
    const router = useRouter()
    const claimShow = ref(false)
    const mediaData = ref([
      {
        img: require('@/assets/images/media-01.png'),
        link: process.env.VUE_APP_BASE_T_LINK
      },
      {
        img: require('@/assets/images/media-02.png'),
        link: process.env.VUE_APP_BASE_TWITTER_LINK
      },
      {
        img: require('@/assets/images/media-03.png'),
        link: process.env.VUE_APP_BASE_DISCORD_LINK
      },
      {
        img: require('@/assets/images/media-04.png'),
        link: process.env.VUE_APP_BASE_GITHUB_LINK
      }
    ])

    function hardClose (dialog) {
      claimShow.value = dialog
    }
    onMounted(() => { })
    return {
      system,
      bodyWidth,
      claimShow,
      hardClose,
      mediaData
    }
  }
})
</script>

<style lang="less" scoped>
#container-main {
  position: relative;
  height: 100vh;
  font-size: 18px;
  letter-spacing: 1px;
  word-break: break-word;
  line-height: 1;
  .media {
    position: absolute;
    right: 80px;
    bottom: 28px;
    z-index: 9;
    li {
      width: 32px;
      margin: 0 0 0 18px;
      img {
        width: 100%;
        cursor: pointer;
      }
    }
  }
  .module {
    text-align: left;
    h2 {
      padding: 25px 0 55px;
    }
    .el-button {
      height: auto;
      padding: 17px 44px;
      margin: 25px 0 0;
      line-height: 1;
      color: #fff;
      border-radius: 100px;
    }
  }
}
</style>
