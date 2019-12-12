<template>
<div>
  <v-sheet elevation="14">
    <canvas id="cv" ref="canvas"></canvas>
  </v-sheet>
  <a class="btn" id="download">甲村</a>
  <button id="clear">バイバイ( ^_^)/~~~</button>
</div>
</template>

<style lang="css" scoped>
  #cv {
    border: solid 1px black;
  }
</style>

<script>

const cvWidth = 700
const cvHeight = 700
const cvColor = '0,0,0,1'
const cvBold = 1
const bgColor = 'rgb(255,255,255)'

export default {
  data() {
    return {
      cv: null,
      ctx: null,
      dlBtn: null,
      clrBtn: null,
      clickFlg: 0
    }
  },
  mounted() {
      this.cv = document.getElementById('cv')
      this.ctx = this.cv.getContext('2d')

      this.dlBtn = document.getElementById('download')
      this.clrBtn = document.getElementById('clear')

      // canvasの背景色を設定(指定がない場合にjpeg保存すると背景が黒になる)
      this.setBgColor()
      this.setCvSize()

      // canvas上でのイベント
      this.cv.addEventListener('mousedown', () => { this.clickFlg = 1 })
      this.cv.addEventListener('mouseup', () => { this.clickFlg = 0 })
      this.cv.addEventListener('mousemove', (e) => {
        if(!this.clickFlg) return false
        this.draw(e.offsetX, e.offsetY)
      })
      // 描画クリア
      this.clrBtn.addEventListener('click', () => {
        this.ctx.clearRect(0,0,cvWidth,cvHeight)
        this.setBgColor(bgColor)
      })
      // canvasを画像で保存
      this.dlBtn.addEventListener('click', () => {
        this.dlBtn.href = this.cv.toDataURL("image/jpeg")
        this.dlBtn.download = 'komura.jpeg'
      })

  },
  methods: {
    // canvasの背景色を設定(指定がない場合にjpeg保存すると背景が黒になる)
    setBgColor: function(color=bgColor, w=cvWidth, h=cvHeight) {
      this.ctx.fillStyle = color
      this.ctx.fillRect(0,0,w,h)
      console.log(color, w, h)
    },
    // 描画処理
    draw: function(x, y) {
      this.ctx.lineWidth = cvBold
      this.ctx.strokeStyle = 'rgba('+cvColor+')'
      // 初回処理の判定
      if (this.clickFlg == "1") {
        this.clickFlg = "2"
        this.ctx.beginPath()
        this.ctx.lineCap = "round"  //　線を角丸にする
        this.ctx.moveTo(x, y)
      } else {
        this.ctx.lineTo(x, y)
      }
      this.ctx.stroke()
    },
    setCvSize: function(w=cvWidth, h=cvHeight) {
      this.cv.width = w
      this.cv.height = h
      this.cv.parentElement.style.width = w + "px"
      this.cv.parentElement.style.height = h + "px"
    }
  }
}
</script>
