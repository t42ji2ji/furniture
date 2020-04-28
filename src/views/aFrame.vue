<template lang="pug">
  .three
    .view
      a-scene(renderer="antialias: true" embedded=true debug=true)
        a-sky(:src="require('../assets/360image/t2.jpg')" rotation="0 -130 0")
        a-text(font="kelsonsans" value="Puy de Sancy, France" width="6" position="-2.5 1.55 -1.5" rotation="0 45 0")



        //- a-entity(id="rain" particle-system="preset: rain; color: #24CAFF; particleCount: 5000")
        //- a-entity(id="sphere" geometry="primitive: sphere"
        //-           material="color: #EFEFEF; shader: flat"
        //-           position="0 0.15 -5"
        //-           light="type: point; intensity: 5"
        //-           animation="property: position; easing: easeInOutQuad; dir: alternate; dur: 1000; to: 0 -0.10 -5; loop: true")
        //- a-entity(id="ocean" ocean="density: 20; width: 50; depth: 50; speed: 4"
        //-           material="color: #9CE3F9; opacity: 0.75; metalness: 0; roughness: 1"
        //-           rotation="-90 0 0")
        //- a-entity(id="sky" geometry="primitive: sphere; radius: 5000"
        //-           material="shader: gradient; topColor: 235 235 245; bottomColor: 185 185 210"
        //-           scale="-1 1 1")
        //- a-entity(id="light" light="type: ambient; color: #888")

</template>

<script>
// @ is an alias to /src
import * as THREE from "aframe";

export default {
  name: "Home",
  components: {},
  mounted() {},
  data() {
    return {
      camera: null,
      scene: null,
      geometry: null,
      material: null,
      mesh: null,
      renderer: null
    };
  },
  methods: {
    init() {
      this.camera = new THREE.PerspectiveCamera(
        70,
        window.innerWidth / window.innerHeight,
        0.01,
        10
      );
      this.camera.position.z = 1;

      this.scene = new THREE.Scene();

      this.geometry = new THREE.BoxGeometry(0.2, 0.2, 0.2);
      this.material = new THREE.MeshNormalMaterial();

      this.mesh = new THREE.Mesh(this.geometry, this.material);
      this.scene.add(this.mesh);

      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setSize(window.innerWidth, window.innerHeight * 0.9);
      this.$refs.three.appendChild(this.renderer.domElement);
    },

    animate() {
      requestAnimationFrame(this.animate);

      this.mesh.rotation.x += 0.01;
      this.mesh.rotation.y += 0.02;

      this.renderer.render(this.scene, this.camera);
    }
  }
};
</script>

<style lang="scss" scoped>
.three {
  height: 90vh;
  box-sizing: border-box;
}
.view {
  height: 100%;
  padding: 0px 20px;
}
</style>
