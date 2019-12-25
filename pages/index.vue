<template>
  <div class="container">
    <v-layout row>
      <v-flex xs7>
        <v-card elevation="14" class="cv-wrapper">
          <canvas id="cv" ref="canvas"
            @mousedown="cvMousedown()"
            @mouseup="cvMouseup()"
            @mousemove="cvMousemove"
          ></canvas>
          <!-- <canvas id="tmpcv" width="256" height="256"></canvas> -->
        </v-card>
        <div class="btn-group">
          <!-- <v-btn href="javascript:void(0)" class="v-btn info" id="download">保存</v-btn> -->
          <v-btn href="javascript:void(0)" id="generate" class="v-btn info" @click="transfer()">画像生成</v-btn>
          <v-btn href="javascript:void(0)" id="clear" color="blue-grey" dark @click="cvClear()">クリア</v-btn>
          <v-btn href="javascript:void(0)" id="demo" class="v-btn error mt-1" @click="imagePut('./test.png')">デモ用</v-btn>
        </div>

        <v-dialog v-model="genDialog" width="256" height="300">
          <div class="dialog-wrapper2">
            <canvas id="trnsdcv" width="300" height="256"></canvas>
            <v-btn class="info mx-5" id="gen">生成</v-btn>
          </div>
        </v-dialog>
      </v-flex>

      <v-flex xs5 class="menu">
        <v-item-group>
          <v-container pa-0>
            <v-layout wrap>
              <v-flex v-for="n in 12" :key="n" xs6>
                <v-item>
                  <v-hover v-slot:default="{ hover }">
                    <v-card
                      class="d-flex align-center menu-item"
                      :elevation="hover ? 18 : 4"
                      height="275"
                      width="275"
                      @click="dialog = true"
                    >
                      <!-- <v-scroll-y-transition>
                    <div
                      v-if="active"
                      class="display-3 text-xs-center"
                    >
                      Active
                    </div>
                      </v-scroll-y-transition>-->
                    </v-card>
                  </v-hover>
                </v-item>
              </v-flex>
            </v-layout>
          </v-container>
        </v-item-group>

        <v-dialog v-model="dialog" width="500">
          <div class="dialog-wrapper" v-if="dialog">
            <Viwer modelName="./sample.obj"></Viwer>
            <!-- <v-btn class="info">ボタン</v-btn> -->
          </div>
        </v-dialog>
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
  /* border: solid 1px black; */
}

.btn-group {
  text-align: center;
  margin: 10px auto;
}

#download {
  width: 60%;
  padding: 20px;
}

#generate {
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

.dialog-wrapper {
  height: 510px;
  width: 500px;
  background-color: white;
  border: solid 1px black;
}
.dialog-wrapper2 {
  height: 300px;
  width: 256px;
  background-color: white;
  border: solid 1px black;
}
</style>

<script>
import Viwer from "~/components/viewer.vue"

const cvWidth = 256
const cvHeight = 256
const cvColor = "0,0,0,1"
const cvBold = 1
const bgColor = "rgb(255,255,255)"

export default {
  head: {
    script: [{ src: "https://unpkg.com/ml5@0.4.3/dist/ml5.min.js" }]
  },
  data() {
    return {
      cv: null,
      ctx: null,
      dlBtn: null,
      clrBtn: null,
      genBtn: null,
      demoBtn: null,
      clickFlg: false,
      dialog: false,
      genDialog: false,
      trnsdcv: null,
      trnsdctx: null,
      tmpcv: null,
      tmpctx: null,
      pix2pix: null,
      w: cvWidth,
      h: cvHeight
    }
  },
  mounted() {
    this.cv = document.getElementById("cv")
    this.ctx = this.cv.getContext("2d")

    this.tmpcv = document.getElementById("tmpcv")
    this.tmpctx = this.cv.getContext("2d")

    this.dlBtn = document.getElementById("download")
    this.clrBtn = document.getElementById("clear")
    this.genBtn = document.getElementById("generate")
    this.demoBtn = document.getElementById("demo")

    // canvasの背景色を設定(指定がない場合にjpeg保存すると背景が黒になる)
    this.setCvSize()
    this.setBgColor()

    //pix2pixモデルのロード
    console.log("ml5 ver: ", ml5.version)
    this.pix2pix = ml5.pix2pix("./chair.pict", () => {
      console.log("Model Loaded.")
    })
  },
  methods: {
    // canvasの背景色を設定(指定がない場合にjpeg保存すると背景が黒になる)
    setBgColor: function(color = bgColor, w = cvWidth, h = cvHeight) {
      this.ctx.fillStyle = color
      this.ctx.fillRect(0, 0, w, h)

      this.tmpctx.fillStyle = color
      this.tmpctx.fillRect(0, 0, 256, 256)
    },
    // 描画処理
    draw: function(x, y) {
      this.ctx.lineWidth = cvBold
      this.ctx.strokeStyle = "rgba(" + cvColor + ")"
      // 初回処理の判定
      if (this.clickFlg == "1") {
        this.clickFlg = "2"
        this.ctx.beginPath()
        this.ctx.lineCap = "round" //　線を角丸にする
        this.ctx.moveTo(x, y)
      } else {
        this.ctx.lineTo(x, y)
      }
      this.ctx.stroke()
    },
    setCvSize: function(w = cvWidth, h = cvHeight) {
      this.cv.width = w
      this.cv.height = h
      this.cv.parentElement.style.width = w + "px"
      this.cv.parentElement.style.height = h + "px"
      document.getElementsByClassName("btn-group")[0].style.width = w + "px"

    },
    imagePut(img="./test.png") {
      console.log(img)
      let imgTest = new Image()
      imgTest.src = img

      this.ctx.drawImage(imgTest, 0, 0, 256, 256)
    },
    transfer() {
      // let image = this.ctx.getImageData(0, 0, cvWidth, cvHeight)
      // this.tmpctx.putImageData(image, 0, 0)
      // console.log(this.tmpcv.toDataURL("image/jpeg"))
      this.genDialog = true
      console.log('transfer')
      // エレメントが描写されるまで待つ
      setTimeout(() => {
        this.trnsdcv = document.getElementById("trnsdcv")
        this.trnsdctx = this.trnsdcv.getContext("2d")

        this.pix2pix.transfer(this.cv, (err, result) => {
          console.log(result)
          this.trnsdctx.drawImage(result, 0, 0, 256, 256)
        })
        // this.trnsdctx.drawImage(image, 0, 0, 650, 650, 0, 0, 256, 256)
      }, 500)
    },
    cvMousedown() { this.clickFlg = 1 },
    cvMouseup() { this.clickFlg = 0 },
    cvMousemove(e) {
      if (!this.clickFlg)
        return false
      this.draw(e.offsetX, e.offsetY)
    },
    cvClear() {
      this.ctx.clearRect(0, 0, cvWidth, cvHeight)
      this.setBgColor(bgColor)
    },
    // cvDownload() {
    //   this.dlBtn.href = this.cv.toDataURL("image/jpeg")
    //   this.dlBtn.download = 'komura.jpeg'
    // }
  },
  components: {
    Viwer
  }
};
</script>
