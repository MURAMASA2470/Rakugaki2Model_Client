<template>
<div class="container">
  <v-layout row>

    <v-flex xs7>
      <v-sheet elevation="14" class="cv-wrapper">
        <canvas id="cv" ref="canvas"></canvas>
      </v-sheet>
      <div class="btn-group">
        <!-- <n-link to="/" class="v-btn" id="download">甲村</n-link> -->
        <v-btn href=" " class="v-btn info" id="download">保存</v-btn>
        <v-btn href=" " id="clear" color="blue-grey" dark>クリア</v-btn>
      </div>
    </v-flex>

    <v-flex xs5 class="menu">
       <v-item-group>
        <v-container pa-0>
          <!-- <v-layout wrap> -->
            <v-flex
              v-for="n in 12"
              :key="n"
              xs12
            >
              <v-item>
                <v-card
                  slot-scope="{ active, toggle }"
                  :color="active ? 'primary' : ''"
                  class="d-flex align-center menu-item"
                  dark
                  height="100"
                  width="90%"
                  @click="toggle"
                >
                  <v-scroll-y-transition>
                    <div
                      v-if="active"
                      class="display-3 text-xs-center"
                    >
                      Active
                    </div>
                  </v-scroll-y-transition>
                </v-card>
              </v-item>
            </v-flex>
          <!-- </v-layout> -->
        </v-container>
      </v-item-group>
    </v-flex>

  </v-layout>
</div>
</template>

<style lang="css" scoped>
.container {
  max-width: 100vw;
}

.cv-wrapper {
  margin: 0 auto;
}

  #cv {
    border: solid 1px black;
  }

  .btn-group {
    text-align: center;
    margin: 10px auto;
  }

    #download {
      width: 60%;
      padding: 20px;
    }

    #clear {
      width: 30%;
      padding: 20px;
    }

  .menu {
    max-height: 700px;
    overflow: auto;
  }

    .menu-item {
      margin: 10px auto;
    }

    .menu-item:first-child {
      margin-top: 0;
    }
</style>

<script>

const cvWidth = 650
const cvHeight = 650
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
      this.setCvSize()
      this.setBgColor()

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
      document.getElementsByClassName("btn-group")[0].style.width = w + "px"
    }
  }
}
</script>
