<template>
  <canvas id="canvas" width="500" height="450"></canvas>
</template>

<script>
import * as THREE from "three";
import {MTLLoader, OBJLoader} from 'three-obj-mtl-loader'

export default {
  data() {
    const scene = new THREE.Scene();
    const renderer = null;
    const camera = new THREE.PerspectiveCamera(75, 600 / 400, 0.1, 1000);
    const light = new THREE.DirectionalLight(0xffffff);
    // const geometry = new THREE.BoxGeometry(1, 1, 1);
    // const material = new THREE.MeshNormalMaterial();
    // const cube = new THREE.Mesh(geometry, material);
    // OBJLoader.OBJLoader(THREE)
    let loader = new OBJLoader();
    let model;
    return { scene, renderer, camera, light, /*geometry, material, cube,*/ loader, model };
  },
  async mounted() {
    this.objRender()

  },
  methods: {
    animate() {
      requestAnimationFrame(this.animate);

      this.model.rotation.y += 0.01;

      this.renderer.render(this.scene, this.camera);
    },
    objRender() {
      const $canvas = document.getElementById("canvas");
    // canvasを後付けで設定する方法あったら教えてほしいー
    this.renderer = new THREE.WebGLRenderer({
      antialias: true,
      canvas: $canvas
    });
    this.renderer.setSize(500, 450)

    let self = this

    let task = new Promise((resolve, reject) => {
      self.loader.load(this.modelName, function(object){
        let model = object.clone();
        model.lookAt(0,9,0)
        model.scale.set(4, 4, 4);            // 縮尺の初期化
        model.rotation.set(0, 5, 0);         // 角度の初期化
        model.position.set(0.5, 0, 0.1);         // 位置の初期化

        let obj = new THREE.Object3D();
        obj.add(model);
        self.scene.add(obj);                     // sceneに追加
        self.model = model
        console.log(1)
        resolve(model)
      }, (progress) => {}, (error) => {});
    })

    task.then((model) => {
      return new Promise((resolve, reject) => {
        self.camera.position.set(0, 0.5, 2.5);
        self.light.position.set(0, 0, 10);
        // self.scene.add(self.cube);
        self.scene.add(self.light);

        self.animate();
        resolve(null)
      })
    }).catch((e) => {
      console.log(e)
    })
    }
  },
  props: {
    modelName: String,
    width: Number,
    height: Number
  }
};
</script>