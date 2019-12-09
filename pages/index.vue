<template>
<div>
  <h1>oooon</h1>
  <canvas id="cv" ref="canvas"></canvas>
</div>
</template>

<style lang="css" scoped>
  #cv {
    width: 600px;
    height: 600px;
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
      let context = cv.getContext('2d')

      console.log(context)
  }
}
</script>
