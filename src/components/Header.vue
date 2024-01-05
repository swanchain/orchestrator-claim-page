<template>
  <section id="container-header">
    <div class="header lang-max flex-row">
      <img :src="swanLogo" class="logo-img mit" alt='swan' @click="goMain" />
      <!-- <el-input v-model="searchValue" class="w-50 m-2 small-min" placeholder="search models,datasets,users..." /> -->
    </div>
  </section>
</template>
<script>
import { defineComponent, computed, onMounted, onBeforeUnmount, nextTick, watch, ref, reactive, getCurrentInstance } from 'vue'
import { useStore } from "vuex"
import { useRouter, useRoute } from 'vue-router'
// import {} from '@element-plus/icons-vue'
// import { ElInput } from "element-plus"
export default defineComponent({
  // components: { ElInput },
  setup () {
    const store = useStore()
    const bodyWidth = ref(document.body.clientWidth < 992)
    const system = getCurrentInstance().appContext.config.globalProperties
    const route = useRoute()
    const router = useRouter()
    const swanLogo = require("@/assets/images/logo.png")
    const searchValue = ref('')

    onMounted(() => { })
    watch(route, (to, from) => { })
    return {
      swanLogo,
      searchValue
    }
  }
})
</script>
<style lang="less" scoped>
#container-header {
  :deep(.header) {
    flex-wrap: nowrap;
    margin: auto;
    padding: 15px 0;
    @media screen and (min-width: 2160px) {
      padding: 8px 0;
    }
    @media screen and (max-width: 600px) {
      padding: 30px 0;
    }
    .logo-img,
    .mit-body {
      height: 45px;
      cursor: pointer;
      @media screen and (min-width: 2160px) {
        height: 55px;
      }
      @media screen and (min-width: 3000px) {
        height: 65px;
      }
      @media screen and (min-width: 4600px) {
        height: 75px;
      }
      @media screen and (min-width: 5120px) {
        height: 85px;
      }
      @media screen and (min-width: 6000px) {
        height: 95px;
      }
      @media screen and (max-width: 768px) {
        height: 40px;
      }
    }
    .el-input {
      max-width: 386px;
      margin: 0 30px;
      @media screen and (max-width: 1260px) {
        max-width: 500px;
      }
      @media screen and (max-width: 768px) {
        max-width: 600px;
      }
      @media screen and (max-width: 767px) {
        max-width: 100%;
      }
      .el-input__wrapper {
        height: auto;
        padding: 4px 12px 4px 45px;
        background: transparent url(../assets/images/icon_1.png) no-repeat 12px
          center;
        background-size: 20px;
        border-radius: 15px;
        border-color: #d4d4d8;
        color: #fff;
        line-height: 1;
        @media screen and (max-width: 1260px) {
          padding: 8px 12px 8px 50px;
          background-size: 25px;
          border-radius: 20px;
        }
        @media screen and (max-width: 768px) {
          padding: 12px 20px 12px 55px;
          background-size: 27px;
          background-position-x: 15px;
          border-radius: 30px;
        }
        @media screen and (max-width: 767px) {
          height: 100px;
          padding: 0 20px 0 100px;
          background-size: 45px;
          background-position-x: 30px;
          line-height: 100px;
          border-radius: 100px;
        }
        .el-input__inner {
          color: inherit;
        }
      }
      .el-input__prefix {
        padding: 0;
        .el-icon {
          svg {
            width: 22px;
            height: 22px;
            color: #9ea5b3;
          }
        }
      }
    }
  }
}
</style>