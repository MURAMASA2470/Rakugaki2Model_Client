<template>
<div>
  <h1>oooon</h1>
  <canvas id="cv" ref="canvas"></canvas>
  <a class="btn" id="download">甲村</a>
  <button id="clear">バイバイ( ^_^)/~~~</button>
</div>
</template>

<style lang="css" scoped>
  #cv {
    /* width: 500px;
    height: 500px; */
    border: solid 1px black;
  }
</style>

<script>

export default {
  data() {
    return {

    }
  },
  mounted: () => {
      let cv = document.getElementById('cv')
      let ctx = cv.getContext('2d')

      let dlBtn = document.getElementById('download')
      let clrBtn = document.getElementById('clear')
      

      console.log(ctx)

    // 変数宣言
    const cvWidth = 500;
    const cvHeight = 500;
    let cvColor = "0, 0, 0, 1";  // 線の色
    let cvBold = 1;  // 線の太さ
    let clickFlg = 0;  // クリック中の判定 1:クリック開始 2:クリック中
    let bgColor = "rgb(255,255,255)";

    // canvasの背景色を設定(指定がない場合にjpeg保存すると背景が黒になる)
    setBgColor()

    cv.width = cvWidth
    cv.height = cvHeight

    // canvas上でのイベント
    cv.addEventListener('mousedown', () => { clickFlg = 1 })
    cv.addEventListener('mouseup', () => { clickFlg = 0 })
    cv.addEventListener('mousemove', (e) => {
      if(!clickFlg)
        return false;
      draw(e.offsetX, e.offsetY)
    })

    // $("#canvas").mousedown(function(){
    //   clickFlg = 1; // マウス押下開始
    // }).mouseup(function(){
    //   clickFlg = 0; // マウス押下終了
    // }).mousemove(function(e){
    //   // マウス移動処理
    //   if(!clickFlg) return false;
    //   draw(e.offsetX, e.offsetY);
    // });

    // 描画処理
    function draw(x, y) {
      ctx.lineWidth = cvBold;
      ctx.strokeStyle = 'rgba('+cvColor+')';
      // 初回処理の判定
      if (clickFlg == "1") {
        clickFlg = "2";
        ctx.beginPath();
        ctx.lineCap = "round";  //　線を角丸にする
        ctx.moveTo(x, y);
      } else {
        ctx.lineTo(x, y);
      }
      ctx.stroke();
    };

    // 描画クリア
    clrBtn.addEventListener('click',() => {
      ctx.clearRect(0,0,cvWidth,cvHeight);
      setBgColor();
    });

    // // canvasを画像で保存
    dlBtn.addEventListener('click', function(){
      dlBtn.href = cv.toDataURL("image/jpeg")
      dlBtn.download = 'komura.jpeg'
    });

    function setBgColor(){
      // canvasの背景色を設定(指定がない場合にjpeg保存すると背景が黒になる)
      ctx.fillStyle = bgColor;
      ctx.fillRect(0,0,cvWidth,cvHeight);
    }

  }
}
</script>
