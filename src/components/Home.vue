<template>
  <div class="wrapper" id="wrapper" ref="area">
    <el-container>
      <el-header>
        <v-head></v-head>
      </el-header>
      <el-main>
        <div class="content">
          <router-view v-slot="{ Component }">
            <transition name="fade" mode="out-in">
              <keep-alive>
                <component :is="Component" />
              </keep-alive>
            </transition>
          </router-view>
          <el-backtop :right="20" :bottom="50" />
        </div>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import vHead from './Header.vue';
import { defineComponent, computed, onMounted, watch, ref, reactive, getCurrentInstance } from 'vue'
import { useStore } from "vuex"
import { useRouter, useRoute } from 'vue-router'
import { ElBacktop, ElContainer, ElHeader, ElMain } from "element-plus"
export default defineComponent({
  setup () {
    const store = useStore()
    const bodyWidth = ref(document.body.clientWidth < 992)
    const system = getCurrentInstance().appContext.config.globalProperties
    const route = useRoute()
    const router = useRouter()

    onMounted(() => { })
    return {}
  },
  components: {
    vHead, ElBacktop, ElContainer, ElHeader, ElMain
  }
})
</script>

<style lang="less" scoped>
.wrapper {
  min-height: 100vh;
  .content {
    :deep(.el-backtop) {
      background-color: #ffffff;
      color: #231f20;
      box-shadow: 0 0 15px rgba(0, 0, 0, 0.35);
      @media screen and (max-width: 768px) {
        width: 80px;
        height: 80px;
      }
      @media screen and (max-width: 600px) {
        width: 110px;
        height: 110px;
      }
      .el-icon,
      svg {
        @media screen and (max-width: 768px) {
          width: 50px;
          height: 50px;
        }
        @media screen and (max-width: 600px) {
          width: 80px;
          height: 80px;
        }
      }
    }
  }
  .el-container {
    .el-header,
    .el-footer,
    .el-main {
      width: 100%;
      height: auto;
      padding: 0;
    }
    .el-header {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      z-index: 99;
    }
  }
}
</style>

<style lang="less">
// .fade-enter-active,
// .fade-leave-active {
//   transition: opacity 0.3s;
// }

// .fade-enter-from,
// .fade-leave-to {
//   opacity: 0;
// }

.mt-border {
  margin: 30px auto;
  border-top: 1px solid #8c878d;
}
.lang-max {
  width: 80%;
  max-width: 1540px;
  padding: 0;
  margin: auto;
  @media screen and (min-width: 2160px) {
    width: 100%;
    max-width: 1680px;
  }
  @media screen and (min-width: 2664px) {
    max-width: 1880px;
  }
  @media screen and (min-width: 4090px) {
    max-width: 2120px;
  }
  @media screen and (min-width: 5120px) {
    max-width: 2360px;
  }
  @media screen and (max-width: 1200px) {
    width: calc(100% - 64px);
    padding: 0 32px;
  }
}

.wrapper {
  .font-53 {
    font-size: 53px;
    @media screen and (min-width: 2160px) {
      font-size: 27px;
    }
    @media screen and (max-width: 600px) {
      font-size: 106px;
    }
  }

  .font-52 {
    font-size: 52px;
    @media screen and (min-width: 2160px) {
      font-size: 26px;
    }
    @media screen and (max-width: 600px) {
      font-size: 104px;
    }
  }

  .font-41 {
    font-size: 41px;
    @media screen and (min-width: 2160px) {
      font-size: 20px;
    }
    @media screen and (max-width: 600px) {
      font-size: 82px;
    }
  }

  .font-29 {
    font-size: 29px;
    @media screen and (min-width: 2160px) {
      font-size: 20px;
    }
    @media screen and (max-width: 600px) {
      font-size: 58px;
    }
  }

  .font-24 {
    font-size: 24px;
    @media screen and (min-width: 2160px) {
      font-size: 18px;
    }
    @media screen and (max-width: 768px) {
      font-size: 36px;
    }
    @media screen and (max-width: 600px) {
      font-size: 60px;
    }
  }

  .font-23 {
    font-size: 23px;
    @media screen and (min-width: 2160px) {
      font-size: 11px;
    }
    @media screen and (max-width: 600px) {
      font-size: 46px;
    }
  }

  .font-22 {
    font-size: 22px;
    @media screen and (min-width: 2160px) {
      font-size: 17px;
    }
    @media screen and (max-width: 768px) {
      font-size: 33px;
    }
    @media screen and (max-width: 600px) {
      font-size: 55px;
    }
  }

  .font-20 {
    font-size: 20px;
    @media screen and (min-width: 2160px) {
      font-size: 16px;
    }
    @media screen and (max-width: 768px) {
      font-size: 30px;
    }
    @media screen and (max-width: 600px) {
      font-size: 60px;
    }
  }

  .font-18 {
    font-size: 18px;
    @media screen and (min-width: 2160px) {
      font-size: 15px;
    }
    @media screen and (max-width: 768px) {
      font-size: 26px;
    }
    @media screen and (max-width: 600px) {
      font-size: 54px;
    }
  }

  .font-16 {
    font-size: 16px;
    @media screen and (min-width: 2160px) {
      font-size: 14px;
    }
    @media screen and (max-width: 768px) {
      font-size: 24px;
    }
    @media screen and (max-width: 600px) {
      font-size: 48px;
    }
  }
  .font-14 {
    font-size: 14px;
    @media screen and (min-width: 2160px) {
      font-size: 13px;
    }
    @media screen and (max-width: 1260px) {
      font-size: 18px;
    }
    @media screen and (max-width: 768px) {
      font-size: 28px;
    }
    @media screen and (max-width: 600px) {
      font-size: 42px;
    }
  }
}

.font-black {
  font-family: "Montserrat-black";
}
.font-bold {
  font-family: "Montserrat-bold";
}

.weight-3 {
  font-weight: 300;
}
.weight-4 {
  font-weight: 400;
}
.weight-5 {
  font-weight: 500;
}
.weight-6 {
  font-weight: 600;
}
.weight-7 {
  font-weight: 700;
}
.weight-8 {
  font-weight: 800;
}

.flex-row {
  display: flex !important;
  align-items: center;
  flex-wrap: wrap;
  &.space-between {
    justify-content: space-between;
  }
  &.center {
    justify-content: center;
  }
  &.flex-start {
    justify-content: flex-start;
  }
  &.flex-end {
    justify-content: flex-end;
  }
  &.nowrap {
    flex-wrap: nowrap;
  }
}

.img {
  display: block;
  width: 100%;
}

.picture-mit {
  max-width: 170px;
  margin: auto;
}

.menu-width-mit {
  width: calc(100% - 230px);
  @media screen and (max-width: 540px) {
    width: calc(100% - 200px);
  }
}

.el-loading-spinner .path {
  stroke: #4b66fc;
}
.el-loading-spinner .el-loading-text {
  margin: 15px 0;
  font-size: 20px;
  color: #4b66fc;
  @media screen and (min-width: 2160px) {
    font-size: 16px;
  }
  @media screen and (max-width: 768px) {
    font-size: 30px;
  }
  @media screen and (max-width: 600px) {
    font-size: 60px;
  }
}
</style>

