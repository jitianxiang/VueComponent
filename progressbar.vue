<template>
  <div class="progress-bar" ref="progressbar" @click.stop="progressClick">
    <div class="bar-inner">
      <div class="progress" ref="progress"></div>
      <div class="progress-btn-wrapper" ref="progressbtn"
           @touchstart.prevent="progressTouchStart"
           @touchmove.prevent="progressTouchMove"
           @touchend.prevent="progressTouchEnd">
        <div class="progress-btn"></div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    name: 'progressbar',
    data() {
      return {}
    },
    props: {
      percent: {
        type: Number,
        default: 0
      }
    },
    watch: {
      percent(val) {
        if (val >= 0 && !this.touch.initiated) {
          const barWidth = this.$refs.progressbar.clientWidth - 16
          const offsetWidth = val * barWidth
          this._offset(offsetWidth)
        }
      }
    },
    created() {
      this.touch = {}
    },
    components: {},
    computed: {},
    methods: {
      progressTouchStart(e) {
        this.touch.initiated = true
        this.touch.startX = e.touches[0].pageX
        this.touch.left = this.$refs.progress.clientWidth
      },
      progressTouchMove(e) {
        if (!this.touch.initiated) return
        const deltaX = e.touches[0].pageX - this.touch.startX
        //滑动过程中计算按钮和进度条的偏移量
        const offsetWidth = Math.min(this.$refs.progressbar.clientWidth - 16, Math.max(0, this.touch.left + deltaX))
        this._offset(offsetWidth)
      },
      progressTouchEnd(e) {
        this.touch.initiated = false
        this._triggerPercent()
      },
      _offset(offsetWidth) {
        this.$refs.progress.style.width = offsetWidth + 'px'
        this.$refs.progressbtn.style.transform = `translate3d(${offsetWidth}px,0,0)`
        this.$refs.progressbtn.style.webkitTransform = `translate3d(${offsetWidth}px,0,0)`
      },
      //将拖动结束后的位置通知父组件，以便同步音乐播放进度
      _triggerPercent() {
        const barWidth = this.$refs.progressbar.clientWidth - 16
        const percent = this.$refs.progress.clientWidth / barWidth
        this.$emit('percentChange', percent)
      },
      progressClick(e) {
        const rect = this.$refs.progressbar.getBoundingClientRect()
        const offsetWidth = e.pageX - rect.left
        this._offset(offsetWidth)
        this._triggerPercent()
      }
    },
    mounted() {

    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='scss' scoped>

  .progress-bar {
    height: .3rem;
    .bar-inner {
      position: relative;
      top: .13rem;
      height: .04rem;
      background: rgba(0, 0, 0, 0.3);
      .progress {
        position: absolute;
        height: 100%;
        background: #ffcd32;
      }

      .progress-btn-wrapper {
        position: absolute;
        left: -.08rem;
        top: -.13rem;
        width: .3rem;
        height: .3rem;
        .progress-btn {
          position: relative;
          top: .07rem;
          left: .07rem;
          box-sizing: border-box;
          width: .16rem;
          height: .16rem;
          border: .03rem solid #fff;
          border-radius: 50%;
          background: #ffcd32;
        }
      }
    }
  }
</style>
