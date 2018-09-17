<template>
  <div>
    <div class="guaguale-box">
      <span v-if="!src">{{msg}}</span>
      <img :src="src" alt="" v-if="src">
      <canvas ref="canvas" width="200" height="100" class="canvas"></canvas>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'guaguale',
    data() {
      return {
        msg: '谢谢惠顾',
        src: require('../assets/babef.png')
      }
    },
    computed: {
      ctx() {
        return this.$refs.canvas.getContext('2d');
      },
      canvas() {
        return this.$refs.canvas;
      }
    },
    mounted() {
      this.init()
    },
    methods: {
      init() {
        this.ctx.fillStyle = '#ccc'
        this.ctx.fillRect(0, 0, 200, 100);
        this.ctx.globalCompositeOperation = 'destination-out';
        this.initEvent()
      },
      initEvent() {
        this.canvas.addEventListener('touchmove', (e) => {
          e.preventDefault();
          let touchData = e.touches[0]
          let l = e.currentTarget.getBoundingClientRect()
          this.drawArc(touchData, l)
          this.checkOver()
        })
      },
      drawArc(touchData, l) {
        this.ctx.beginPath();
        this.ctx.arc(touchData.clientX - l.left, touchData.clientY - l.top, 10, 0, Math.PI * 2, true);
        this.ctx.fill()
      },
      checkOver() {
        let imgData = this.ctx.getImageData(0, 0, 200, 100)
        let data = imgData.data;
        let long = data.length
        let count = 0;
        for (let i = 0; i < long; i += 4) {
          let alpha = data[i + 3];
          if (alpha < 10) {
            count++;
          }
        }

        let percent = count/(long/4);
        if(percent>0.5){
          alert('抽奖完成')
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .guaguale-box {
    position: absolute;
    left: 0;
    top: 0px;
    right: 0;
    bottom: 0px;
    margin: auto;
    width: 200px;
    height: 100px;
    background: rgba(0, 0, 0, 0.1);
    border-radius: 3px;
    font-size: 28px;
    text-align: center;
    line-height: 100px;
    z-index: 999;
    > img {
      position: absolute;
      left: 0;
      top: 0px;
      right: 0;
      bottom: 16px;
      margin: auto;
      width: 100%;
    }
    .canvas {
      position: absolute;
      left: 0;
      top: 0px;
      right: 0;
      bottom: 0px;
      margin: auto;
    }
  }
</style>
