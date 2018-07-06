<template>
  <transition name="confirm-fade">
    <div class="confirm" v-show="showFlag">
      <div class="confirm-wrapper">
        <div class="confirm-content">
          <p class="text">提示</p>
          <p class="text" style="font-size: .15rem;padding-top: 0">{{details}}</p>
          <div class="operate">
            <span class="btn" @click.stop="cancel">{{cancelBtnText}}</span>
            <span class="btn" @click.stop="confirm">{{confirmBtnText}}</span>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  export default {
    name: 'confirm',
    data() {
      return {
        showFlag: false
      }
    },
    props: {
      title: {
        type: String,
        default: '提示'
      },
      details: {
        type: String,
        default: '您确定要关闭视频软件吗？'
      },
      confirmBtnText: {
        type: String,
        default: '确定'
      },
      cancelBtnText: {
        type: String,
        default: '取消'
      }
    },
    components: {},
    computed: {},
    methods: {
      show() {
        this.showFlag = true
      },
      hide() {
        this.showFlag = false
      },
      cancel() {
        this.hide()
        this.$emit('cancel')
      },
      confirm() {
        this.hide()
        this.$emit('confirm')
      }
    },
    mounted() {

    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='scss' scoped>

  .confirm {
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    z-index: 998;
    background-color: rgba(0, 0, 0, 0.3);

    &.confirm-fade-enter-active {
      animation: confirm-fadein 0.3s;
      .confirm-content {
        animation: confirm-zoom 0.3s;
      }
    }

    .confirm-wrapper {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      z-index: 999;

      .confirm-content {
        width: 3.8rem;
        height: 1.48rem;
        border-radius: .03rem;
        background: #424342;

        .text {
          padding: .19rem .25rem;
          line-height: .2rem;
          font-size: .2rem;
          color: white;
        }

        .operate {
          position: relative;
          height: .5rem;
          font-size: 0;
          .btn {
            position: absolute;
            color: #596C65;
            font-size: .17rem;

            &:nth-child(1) {
              right: 1rem;
            }

            &:nth-child(2) {
              right: .3rem;
            }
          }
        }
      }
    }
  }

  @keyframes confirm-fadein {
    0% {
      opacity: 0;
    }
    100% {
      opacity: 1
    }
  }

  @keyframes confirm-zoom {
    0% {
      transform: scale(0);
    }
    50% {
      transform: scale(1.1);
    }
    100% {
      transform: scale(1);
    }
  }

</style>
