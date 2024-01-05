<template>
  <div class="pop-up flex-row" v-show="props.claimShow">
    <div class="connect-area">
      <div class="close" @click="closeHandle">
        <el-icon>
          <CircleClose />
        </el-icon>
      </div>
      <div class="connect-wallet" v-if="active === 'connect'">
        <div class="title font-24">Connect Wallet</div>
        <div class="metamask flex-row" @click="isLogin">
          <img :src="metaLogo" alt="" class="image" />
          <div class="font-22">MetaMask</div>
        </div>
      </div>

      <div class="connect-result flex-row" v-else-if="active === 'failed'">
        <div class="image">
          <img :src="promptFailed" alt="" class="image" />
        </div>
        <div class="content">
          <div class="title failed font-29">FAILED</div>
          <div class="font-22">
            You are not eligible for rewards currently. Please stay tuned for
            upcoming activities.
          </div>
        </div>
      </div>

      <div class="connect-result flex-row" v-else-if="active === 'success'">
        <div class="image">
          <img :src="promptSuccess" alt="" class="image" />
        </div>
        <div class="content">
          <div class="title success font-29">SUCCESSFUL</div>
          <div class="font-22">
            Reward successfully claimed! View your
            <span @click="system.$commonFun.goLink(hash)">transaction</span>
            , Check your NFT on
            <span
              @click="
                system.$commonFun.goLink(
                  'https://opensea.io/collection/lagrange-nft',
                )
              "
            >
              OpenSea
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {
  defineComponent,
  computed,
  onMounted,
  onActivated,
  onDeactivated,
  watch,
  ref,
  reactive,
  getCurrentInstance,
  toRefs,
  nextTick,
} from 'vue'
import { useStore } from 'vuex'
import { useRouter, useRoute } from 'vue-router'
import { CircleClose } from '@element-plus/icons-vue'
import { ElIcon } from 'element-plus'
import { showLoading, hideLoading } from '@/plugins/loading'
import spookyABI from '@/utils/abi/SpookyProxy.json'
export default defineComponent({
  name: 'Popup',
  components: {
    CircleClose,
    ElIcon,
  },
  props: {
    claimShow: { type: Boolean, default: false },
  },
  setup(props, context) {
    const store = useStore()
    const bodyWidth = ref(document.body.clientWidth <= 768 ? 30 : 50)
    const system = getCurrentInstance().appContext.config.globalProperties
    const route = useRoute()
    const router = useRouter()
    const metaLogo = require('@/assets/images/metamask.png')
    const promptSuccess = require('@/assets/images/prompt-success.png')
    const promptFailed = require('@/assets/images/prompt-failed.png')
    const active = ref('connect')
    const spookyLoad = ref(false)
    const hash = ref('')
    const tokenID = Number(process.env.VUE_APP_TOKENID)
    const spookyAddress = process.env.VUE_APP_CONTACT_ADDRESS
    const spookyContract = new system.$commonFun.web3Init.eth.Contract(
      spookyABI,
      spookyAddress,
    )

    function closeHandle() {
      context.emit('hardClose', false)
    }

    let lastTime = 0
    async function throttle() {
      // Prevent multiple signatures
      let now = new Date().valueOf()
      if (lastTime > 0 && now - lastTime <= 2000) return false
      lastTime = now
      return true
    }

    async function isLogin() {
      const time = await throttle()
      if (!time) return false
      system.$commonFun.Init(async (addr, chain) => {
        if (chain) spookyMethod()
      })
    }

    async function spookyMethod() {
      showLoading()
      try {
        let gasLimit = await spookyContract.methods
          .claim()
          .estimateGas({ from: store.state.metaAddress })

        const tx = await spookyContract.methods
          .claim()
          .send({ from: store.state.metaAddress, gasLimit: gasLimit })
          .on('transactionHash', async (transactionHash) => {
            console.log('transactionHash:', transactionHash)
            hash.value = `${process.env.VUE_APP_POLYGONBLOCKURL}/tx/${transactionHash}`
            active.value = 'success'
            hideLoading()
          })
          .on('error', () => hideLoading())
      } catch (err) {
        console.log('err', err)
        if (err && err.message)
          system.$commonFun.messageTip('error', err.message)
        active.value = 'failed'
        hideLoading()
      }
    }
    onMounted(() => {})
    return {
      system,
      bodyWidth,
      props,
      metaLogo,
      active,
      hash,
      spookyLoad,
      promptSuccess,
      promptFailed,
      closeHandle,
      isLogin,
    }
  },
})
</script>

<style lang="less" scoped>
.pop-up {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  justify-content: center;
  z-index: 99;
  background-color: rgba(0, 0, 0, 0.5);
  color: #fff;
  line-height: 1;
  transition: all 0.2s;
  .connect-area {
    position: relative;
    width: calc(29% - 40px);
    padding: 0 20px;
    background-color: #1b2224;
    border: 1px solid #4c66ff;
    border-radius: 10px;
    @media screen and (max-width: 1260px) {
      width: 35%;
    }
    @media screen and (max-width: 768px) {
      width: 50%;
      padding: 0 30px;
      border-radius: 20px;
    }
    @media screen and (max-width: 600px) {
      width: 70%;
      padding: 0 40px;
      border-radius: 30px;
    }
    @media screen and (max-width: 540px) {
      width: 80%;
    }
    .close {
      position: absolute;
      right: 10px;
      top: 10px;
      font-size: 25px;
      color: #4d73ff;
      cursor: pointer;
      z-index: 100;
      @media screen and (max-width: 1260px) {
        font-size: 30px;
      }
      @media screen and (max-width: 1024px) {
        font-size: 35px;
      }
      @media screen and (max-width: 992px) {
        top: 15px;
        right: 15px;
        font-size: 40px;
      }
      @media screen and (max-width: 768px) {
        top: 20px;
        right: 20px;
        font-size: 45px;
      }
      @media screen and (max-width: 600px) {
        top: 25px;
        right: 25px;
        font-size: 55px;
      }
      @media screen and (max-width: 540px) {
        font-size: 65px;
      }
    }
    .connect-wallet {
      padding: 46px 0;
      @media screen and (max-width: 768px) {
        padding: 69px 0;
      }
      @media screen and (max-width: 600px) {
        padding: 92px 0;
      }
      .title {
        padding: 0 0 15px;
        font-weight: 600;
        @media screen and (max-width: 992px) {
          padding: 0 0 22px;
        }
        @media screen and (max-width: 768px) {
          padding: 0 0 30px;
        }
      }
      .metamask {
        //   width: 490px;
        padding: 22px 33px;
        background-color: #060e10;
        font-weight: 600;
        border-radius: 10px;
        cursor: pointer;
        @media screen and (max-width: 992px) {
          padding: 33px 49px;
        }
        @media screen and (max-width: 768px) {
          padding: 44px 66px;
          border-radius: 20px;
        }
        @media screen and (max-width: 600px) {
          padding: 55px 66px;
          border-radius: 30px;
        }
        .image {
          width: 30px;
          height: auto;
          margin: -2px 20px 0 0;
          @media screen and (max-width: 1260px) {
            width: 35px;
          }
          @media screen and (max-width: 1024px) {
            width: 40px;
          }
          @media screen and (max-width: 992px) {
            width: 45px;
          }
          @media screen and (max-width: 768px) {
            width: 50px;
          }
          @media screen and (max-width: 600px) {
            width: 70px;
            margin: -4px 40px 0 0;
          }
          @media screen and (max-width: 540px) {
            width: 80px;
            margin: -10px 40px 0 0;
          }
        }
      }
    }
    .connect-result {
      align-items: stretch;
      text-align: center;
      line-height: 1.2;
      .image {
        position: relative;
        width: 180px;
        margin: 0;
        img {
          position: absolute;
          left: 0;
          bottom: 20px;
          width: 105%;
          animation: floating-up 3.5s ease-in-out infinite;
        }
        @media screen and (max-width: 768px) {
          width: 280px;
        }
        @media screen and (max-width: 600px) {
          width: 380px;
        }
      }
      .content {
        width: calc(100% - 180px);
        padding: 46px 0;
        @media screen and (max-width: 768px) {
          width: calc(100% - 280px);
          padding: 69px 0;
        }
        @media screen and (max-width: 600px) {
          width: calc(100% - 380px);
          padding: 92px 0;
        }
        .title {
          padding: 0 0 15px;
          font-weight: 600;
          text-transform: uppercase;
          color: #37aa2e;
          @media screen and (max-width: 992px) {
            padding: 0 0 22px;
          }
          @media screen and (max-width: 768px) {
            padding: 0 0 30px;
          }
          &.failed {
            color: #f6555a;
          }
        }
        span {
          color: #4c66ff;
          cursor: pointer;
          &:hover {
            text-decoration: underline;
          }
        }
      }
    }
  }
}
</style>
