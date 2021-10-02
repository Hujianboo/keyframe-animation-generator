<!--
 * @Author: hujianbo
 * @LastEditors: Hujianbo
-->
<!--App.vue-->
<template>
  <div id="app">
    <div class="keyframe-dom">
      <canvas class="keyframe" ref="keyframe" :width="canvasWidth" :height="canvasHeight"></canvas>
    </div>
  </div>
</template>

<script>
import {ref, reactive, defineComponent, onMounted} from '@vue/composition-api';
import BirdPng from './assets/birds.png';
export default {
  name: "App",
  data() {
    return {
      ctx: null,
      canvasWidth: 0,
      canvasHeight: 0,
      row:3,
      column:5,
      image: null,
      spriteArr:[]
    }
  },
  mounted() {
    const canvasDom = this.$refs.keyframe;
    this.ctx = canvasDom.getContext('2d')
    console.log(this.ctx);
    const image = new Image();
    this.image = image;
    image.src = BirdPng;
    image.onload = () => {
      console.log(image);
      this.canvasWidth = Math.floor(image.width / this.column);
      this.canvasHeight = Math.floor(image.height / this.row);
      for(let i = 0; i < this.row; i++) {
        for(let j = 0; j < this.column;j++){
          this.spriteArr.push({
            x: j * this.canvasWidth,
            y: i * this.canvasHeight,
          })
        }
      }
      console.log(this.spriteArr);
      let self = this
      this.$nextTick(() => {
        self.draw()
      })
    }
  },
  methods:{
  draw() {
    let n = 0
    let self = this
    setInterval(() => {
      self.ctx.clearRect(0,0,self.canvasWidth,self.canvasHeight);
      if(n < 14){
        self.ctx.drawImage(self.image,self.spriteArr[n].x,self.spriteArr[n].y,self.canvasWidth,self.canvasHeight,0,0,self.canvasWidth,self.canvasHeight)
        n++;
      }else{
        n = 0;
        self.ctx.drawImage(self.image,self.spriteArr[n].x,self.spriteArr[n].y,self.canvasWidth,self.canvasHeight,0,0,self.canvasWidth,self.canvasHeight)
      }
    }, 50);
  }
  }
};
</script>
<style lang="scss" scoped>
#app{
  height: 100%;
}
</style>
